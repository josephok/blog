---
title: 使用Rust加速Python
date: 2019-03-30 16:23:03
tags: [Rust, Python]
---

`Python`具有开发快速的特点，但是在运行效率上比静态编译型语言慢不少，我们今天要介绍的`Rust`就是其中一种。

    Rust是一种安全、并发、实用的编程语言，有着惊人的运行速度，能够防止段错误，并保证线程安全，使每个人都能够构建可靠、高效的软件。

当我们的`Python`程序出现性能瓶颈时，可以从如下几个方面优化：
1. 优化算法，使用更高效率的算法来提升性能；
2. 使用并发，如多线程程序；
3. 使用编译型语言编写扩展；
4. 优化网络、磁盘、数据库等。

性能优化是个大命题，我们需要从多个方面着手考虑，今天我介绍的是第3种方法，并且选择`Rust`语言。我们将编写一个`so`扩展供`Python`端调用。
这里不会讲`Rust`的入门，具体规范可以看官方文档或者中文文档：https://rustlang-cn.org/

我们选择[Httpbin](https://github.com/postmanlabs/httpbin)作为基准程序，进行修改然后对比效果。

# 1. 原始代码

为了简单，我稍微修改了`view_get`，使之只返回客户端的请求方法。如下：

```json
{
  "method": "GET"
}
```

代码如下：
```py
# httpbin/core.py

@app.route("/get", methods=("GET",))
def view_get():
    """The request's query parameters.
    ---
    tags:
      - HTTP Methods
    produces:
      - application/json
    responses:
      200:
        description: The request's query parameters.
    """

    return jsonify(get_dict("method"))
```

# 2. 测试一下原始代码性能：

使用[wrk](https://github.com/wg/wrk)进行benchmark测试：
```bash
wrk http://127.0.0.1:5000/get -c 400 -t 10

Running 10s test @ http://127.0.0.1:5000/get
  10 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   314.77ms  121.47ms   1.98s    95.24%
    Req/Sec    54.09     37.46   200.00     64.29%
  4355 requests in 10.04s, 1.00MB read
  Socket errors: connect 0, read 0, write 0, timeout 9
Requests/sec:    433.98
Transfer/sec:    101.71KB
```

可以看出：平均时延为315ms左右，RPS为434左右。

# 3. 使用[rust-cpython](https://github.com/dgrunwald/rust-cpython)编写扩展

首先新建一个`lib`类型的项目，比如名字为`handle`：
```bash
cargo new handle --lib
```
这样就在当前项目下新建了一个目录：`handle`，我们需要编辑`handle/Cargo.toml`：

```toml
[package]
name = "handle"
version = "0.1.0"
authors = ["Joseph <josephok@qq.com>"]
edition = "2018"

[lib]
name = "handle"
crate-type = ["cdylib"]

[dependencies.cpython]
version = "0.2"
features = ["extension-module"]
```

然后是编辑`handle/src/lib.rs`:

```rust
#[macro_use]
extern crate cpython;

use cpython::ObjectProtocol;
use cpython::{PyObject, PyResult, Python};
use std::collections::HashMap;

fn ret_py_dict(py: Python, obj: PyObject) -> PyResult<HashMap<&'static str, String>> {
    let mut response = HashMap::new();
    response.insert("method", obj.getattr(py, "method")?.extract(py)?);

    Ok(response)
}

py_module_initializer!(handle, init_handle, PyInit_handle, |py, m| {
    m.add(py, "ret_py_dict", py_fn!(py, ret_py_dict(val: PyObject)))?;

    Ok(())
});
```
这里的`handle`是模块名，`ret_py_dict`就是模块的方法，供`Python`调用，使用方法：
```py
import handle
handle.ret_py_dict(...)
```

然后需要编译此模块，我们使用`Makefile`编写编译规则：
```makefile
build:
        # 编译
	cd handle && cargo build --release
        # 将so文件拷贝到Python代码目录
	cp handle/target/release/libhandle.so httpbin/handle.so
```

执行`make`命令编译并将`so`文件拷贝到指定目录。

# 4. `Python`端调用方法

编写一个`view_get1`方法：

```py
from . import handle

@app.route("/get1", methods=("GET",))
def view_get1():
    """The request's query parameters.
    ---
    tags:
      - HTTP Methods
    produces:
      - application/json
    responses:
      200:
        description: The request's query parameters.
    """
    resp = handle.ret_py_dict(request)
    return jsonify(resp)
```

与我们的原始函数唯一区别是：`jsonify`函数的参数，即响应内容是由扩展模块产生，类型都是`dict`。

`curl`响应为：
```bash
curl localhost:5000/get1

{
  "method": "GET"
}
```

结果一致。

# 5. 测试使用了扩展的性能

```bash
wrk http://127.0.0.1:5000/get1 -c 400 -t 10

Running 10s test @ http://127.0.0.1:5000/get1
  10 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   247.73ms  101.96ms   1.91s    96.28%
    Req/Sec    80.17     53.34   272.00     59.49%
  5482 requests in 10.09s, 1.25MB read
  Socket errors: connect 0, read 0, write 0, timeout 4
Requests/sec:    543.49
Transfer/sec:    127.38KB
```

可以看出：平均时延为248ms左右，RPS为544左右。
比原始版本：时延低70ms，RPS高110，效果比较明显，这里仅仅改写了获取对象属性的方式。

# 6. 总结

如果想继续优化，可以考虑改写`jsonify`函数。在这里我们提出了一个优化`Python`程序性能(Latency, RPS)的方案。前文也说到过，优化性能应该先从代码结构、算法方面做优化，当语言成为瓶颈时，使用`Rust`编写扩展实为一种好的方式。

# 参考：
https://developers.redhat.com/blog/2017/11/16/speed-python-using-rust/