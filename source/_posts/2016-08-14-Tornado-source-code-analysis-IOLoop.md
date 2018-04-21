---
title: Tornado源码解析之IOLoop
date: 2016-08-14 22:33:57
categories: 编程语言
tags: [Python, Tornado]
---

# 0. 简介

[Tornado](https://github.com/tornadoweb/tornado)是一个用Python语言写成的Web服务器兼Web应用框架，由FriendFeed公司在自己的网站FriendFeed中使用，被Facebook收购以后框架以开源软件形式开放给大众。

tornado最大的特点就是其支持异步IO，所以它有着优异的性能。下表是和一些其他Web框架与服务器的对比:(处理器为 AMD Opteron, 主频2.4GHz, 4核) (来源[wikipedia](https://zh.wikipedia.org/wiki/Tornado)


服务|	部署|	请求/每秒
----|----|----
Tornado|    nginx, 4进程|	    8213
Tornado|	1个单线程进程|	3353
Django|	Apache/mod_wsgi|	2223
web.py|	Apache/mod_wsgi|	2066
CherryPy|	独立	|785


先来看看`hello world`的例子。^_^

```python
import tornado.httpserver
import tornado.ioloop
import tornado.options
import tornado.web

from tornado.options import define, options

define("port", default=8888, help="run on the given port", type=int)


class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, world")


def main():
    tornado.options.parse_command_line()
    application = tornado.web.Application([
        (r"/", MainHandler),
    ])
    http_server = tornado.httpserver.HTTPServer(application)
    http_server.listen(options.port)
    tornado.ioloop.IOLoop.current().start()


if __name__ == "__main__":
    main()

```

运行：
```bash
$ python3 helloworld.py
```
我们就得到一个web server监听在8888端口。用curl命令get一下，就返回了"Hello, world"。

tornado的代码结构可以在其官网了解，本文着重分析IOLoop的实现。

# 1. IOLoop

## 1.1 http交互的大致过程
介绍IOLoop之前我们先看看http server和http client交互的一个大致过程。

![http](https://sfault-image.b0.upaiyun.com/108/489/1084890514-57b00bc69d907_articlex)

server端监听在某个端口，client端发送请求过来，server处理后返回，然后继续等待下一个请求，周而复始。如果用socket那一坨来描述的话就是：
```py
1. server.py
================================================================
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(address)
s.listen(backlog)
While True:
    connection = s.accept()
    do_something()
    connection.send()
    connection.close()

2. client.py
=================================================================
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect()
s.send()
s.recv()
s.close()
```

## 1.2 聊聊阻塞与非阻塞
所谓阻塞，就是进程正在等待某些资源（如IO），而处于等待运行的状态（不占用CPU资源）。比如connect(("google.com", 80))返回之前进程都是阻塞的，在它下面的语句得不到执行，除非connect返回。

很显然阻塞式的IO模型有个缺点就是并发量不大，试想如果server进程在`do_something()`处阻塞，而这时另外有个客户端试图连进来，则可能得不到响应。

提高并发量有几种实现方式：多线程（一个连接fork一个线程去处理）；多进程（一个连接fork一个子进程去处理）(apache)；事件驱动(nginx, epoll)等。tornado就是基于epoll(Linux)事件驱动模型实现的。

当然它们有各自的优缺点，此文不详述，有兴趣的读者可以自行google之。^_^

关于IO模型，epoll, 同步，异步，阻塞，非阻塞的概念，可以参考这两篇文章：
https://segmentfault.com/a/1190000003063859

http://blog.csdn.net/historyasamirror/article/details/5778378

## 1.3 IOLoop实现

### 1.3.1 IOLoop配置

前文说到tornado是基于epoll事件驱动模型，也不完全正确，tornado实际上是根据平台选择底层驱动。请看IOLoop类的`configurable_default`方法：
```py
    @classmethod
    def configurable_default(cls):
        if hasattr(select, "epoll"):
            from tornado.platform.epoll import EPollIOLoop
            return EPollIOLoop
        if hasattr(select, "kqueue"):
            # Python 2.6+ on BSD or Mac
            from tornado.platform.kqueue import KQueueIOLoop
            return KQueueIOLoop
        from tornado.platform.select import SelectIOLoop
        return SelectIOLoop
```
这里的IOLoop实际上是个通用接口，根据不同平台选择：linux->epoll，BSD->kqueue，如果epoll和kqueue都不支持则选择select(性能要差些)。

`class IOLoop(Configurable):`IOLoop继承了Configurable类，Configurable类的`__new__`方法调用了`configured_class`方法：

```py
    def __new__(cls, *args, **kwargs):
        base = cls.configurable_base()
        init_kwargs = {}
        if cls is base:
            impl = cls.configured_class()
            if base.__impl_kwargs:
                init_kwargs.update(base.__impl_kwargs)
        else:
            impl = cls
        init_kwargs.update(kwargs)
        instance = super(Configurable, cls).__new__(impl)
        # initialize vs __init__ chosen for compatibility with AsyncHTTPClient
        # singleton magic.  If we get rid of that we can switch to __init__
        # here too.
        instance.initialize(*args, **init_kwargs)
        return instance
```

`configured_class`方法又调用了`configurable_default`方法：

```py
    @classmethod
    def configured_class(cls):
        # type: () -> type
        """Returns the currently configured class."""
        base = cls.configurable_base()
        if cls.__impl_class is None:
            base.__impl_class = cls.configurable_default()
        return base.__impl_class
    ```
所以当初始化一个IOLoop实例的时候就给IOLoop做了配置，根据不同平台选择合适的驱动。

### 1.3.2 IOLoop实例化
下面我们来看IOLoop的实例化函数：
```py
    # Global lock for creating global IOLoop instance
    _instance_lock = threading.Lock()
    @staticmethod
    def instance():
        if not hasattr(IOLoop, "_instance"):
            with IOLoop._instance_lock:
                if not hasattr(IOLoop, "_instance"):
                    # New instance after double check
                    IOLoop._instance = IOLoop()
        return IOLoop._instance
        ```
很显然，这里是实现了一个全局的单例模式。确保多个线程也只有一个IOLoop实例。(思考一下：为什要double check？`if not hasattr(IOLoop, "_instance")` ^_^)

### 1.3.3 实现epoll的接口(假设是在Linux平台)

我们知道epoll支持3种操作：
    EPOLL_CTL_ADD	添加一个新的epoll事件
    EPOLL_CTL_DEL	删除一个epoll事件
    EPOLL_CTL_MOD	改变一个事件的监听方式
分别对应tornado.IOLoop里面的三个函数：`add_handler`, `remove_handler`, `update_handler`

下面看看这三个函数：
```py
    def add_handler(self, fd, handler, events):
        fd, obj = self.split_fd(fd)
        self._handlers[fd] = (obj, stack_context.wrap(handler))
        self._impl.register(fd, events | self.ERROR)

    def update_handler(self, fd, events):
        fd, obj = self.split_fd(fd)
        self._impl.modify(fd, events | self.ERROR)

    def remove_handler(self, fd):
        fd, obj = self.split_fd(fd)
        self._handlers.pop(fd, None)
        self._events.pop(fd, None)
        try:
            self._impl.unregister(fd)
        except Exception:
            gen_log.debug("Error deleting fd from IOLoop", exc_info=True)
```

这里的`self._impl`就是`select.epoll()`，使用方法可以参考epoll接口。

### 1.3.4 事件驱动模型的大致思路
IOLoop的`start()`方法用于启动事件循环（Event Loop）。

```py
(部分源码)
while self._events:
    fd, events = self._events.popitem()
    try:
        fd_obj, handler_func = self._handlers[fd]
        handler_func(fd_obj, events)
    except (OSError, IOError) as e:
        if errno_from_exception(e) == errno.EPIPE:
            # Happens when the client closes the connection
            pass
        else:
            self.handle_callback_exception(self._handlers.get(fd))
    except Exception:
        self.handle_callback_exception(self._handlers.get(fd))
```

大致的思路是：有连接进来（client端请求），就丢给epoll，顺便注册一个事件和一个回调函数，我们主线程还是继续监听请求；然后在事件循环中，如果发生了某种事件（如socket可读，或可写），则调用之前注册的回调函数去处理。这和Node.js的思路是一致的。

### 1.3.5 关于cpu bound任务

tornado很适合处理IO bound的任务，如果遇到cpu bound的任务，则还是会阻塞整个进程。这个时候就必须将耗时的任务丢到另一个worker，或者队列中去处理（如celery）。

### 1.3.6 其他
IOLoop类还有其他一些方法，多为辅助函数，读者可以自行参考，此处不详述。

行文比较草率，如有错误和不足之处，敬请指正。
