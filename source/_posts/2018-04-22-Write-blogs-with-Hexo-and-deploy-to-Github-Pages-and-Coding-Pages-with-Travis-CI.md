---
title: 用Hexo写博客并用Travis CI部署到Github Pages和Coding Pages
date: 2018-04-22 13:40:43
categories: Miscellaneous
tags: [Hexo, Github Pages, Coding Pages, Travis CI]
---

[Hexo](https://hexo.io/zh-cn/)是用Nodejs实现的一个静态网站生成器。简单好用，我们可以使用它来生成我们的博客站点。

## 1. 安装Hexo

首先你必须安装Nodejs环境，如果没有安装的话，请参考其官方文档自行安装。
使用一行命令即可安装Hexo:

```bash
$ npm install -g hexo-cli
```

## 2. 生成站点文件


安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

这里的<folder>替换成你自己的目录，比如我这里是`blog`。

## 3. 配置Hexo

具体网站配置可以参考这里：https://hexo.io/zh-cn/docs/configuration.html
改成你自己想要的配置。

## 4. 写文章

你可以执行下列命令来创建一篇新文章。
```bash
$ hexo new [layout] <title>
```

您可以在命令中指定文章的布局（layout），默认为 `post`，可以通过修改 `_config.yml` 中的 `default_layout` 参数来指定默认布局。

<!-- more -->

## 5. 预览站点

```bash
$ hexo server
```

会启动服务器。默认情况下，访问网址为： `http://localhost:4000/`。

## 6. 生成静态文件

```bash
$ hexo generate
```
运行这条命令后会在当前目录下生成一个新的目录`public`，在这个目录下会生成网站所有的静态文件，可以使用Nginx、Apache这样的web服务器伺服。我们可以利用Github Pages和Coding Pages提供的免费服务来托管我们的网站，不需要额外的服务器。

## 7. 创建Github Pages和Coding Pages需要的repo

我们在Github和Coding上分别创建2个repo。Github上面的repo名称是`<username>.github.io`; Coding上面的repo名称是`<username>.coding.me`。

并且分别开启Pages服务。
Github Pages无需过多设置。Coding Pages需要在代码-〉Pages服务下面打开。

![Coding Pages](https://ws1.sinaimg.cn/large/90b90757gy1fqlfrcpl77j211o0b8q4p.jpg)

## 8. 创建网站源文件repo

 在Github上为我们的源文件（blog目录）创建一个repo。比如repo名称就叫做`blog`。

## 9. 配置部署所需变量

打开`_config.yml`文件，添加如下：

```yml
deploy:
  type: git
  repo:
    github: https://gh_token@github.com/<username>/<username>.github.io.git
    coding: https://<username>:coding_token@git.coding.net/<username>/<username>.coding.me.git
  branch: master
  message: Add post
```

上面的`<username>`替换成你自己的用户名即可。

## 10. 关联到Travis CI

将刚才我们创建的repo添加到Travis CI。
注意开通“Build pushed branches”。

![](https://ws1.sinaimg.cn/large/90b90757gy1fqlhv67jpxj20pp04rjrr.jpg)

并且添加2个环境变量：
`CODING_TOKEN`和`GH_TOKEN`。

![](https://ws1.sinaimg.cn/large/90b90757gy1fqlhept0wvj20sv07hgmf.jpg)

这里的`CODING_TOKEN`和`GH_TOKEN`分别是coding和Github的access token，生成方法分别见官方文档(https://github.com/settings/tokens, https://coding.net/user/account/setting/tokens)，

注意要开通读和写权限。

![](https://ws1.sinaimg.cn/large/90b90757gy1fqlrbasunzj20kl08s0tn.jpg)

![](https://ws1.sinaimg.cn/large/90b90757gy1fqlrce0sd0j20p9093dgp.jpg)

## 11. 配置Travis CI

创建`.travis.yml`文件，内容如下：

```yml
# 指定语言环境
language: node_js
# 指定需要sudo权限
sudo: required

# nodejs版本
node_js:
  - 8.11.1

# 指定缓存模块，可选。缓存可加快编译速度。
cache:
  directories:
    - node_modules

before_install:
  - npm install -g hexo-cli

# Start: Build Lifecycle
install:
  - npm install

# 执行清缓存，生成网页操作
script:
  - hexo clean
  - hexo generate
  # 替换同目录下的_config.yml文件中gh_token字符串为travis后台刚才配置的变量，注意此处sed命令用了双引号。单引号无效！
  - sed -i "s/gh_token/${GH_TOKEN}/g" ./_config.yml
  - sed -i "s/coding_token/${CODING_TOKEN}/g" ./_config.yml
  - hexo deploy

# after_script:
```

上面的`CODING_TOKEN`和`GH_TOKEN`正是我们配置的环境变量。

## 12. push本地目录到Github

完成上述步骤之后，只需要将本地目录push到Github仓库即可。接着Travis CI便会生成静态文件并且部署到Github Pages和Coding Pages上。
