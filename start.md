# 快速开始

hotpack 是简单高效的 web 应用打包工具。 它记录每个源文件的版本，把缓存做到极致，使二次编译获得极高的速度，配置也很简单。

hotpack 不对应用造成任何干扰。开发环境不会对源文件进行合并，会保持源文件的目录结构。
 
npm 安装

``` bash
npm install -g hotpack
```

为了简单高效，hotpack并不打算包法百病。一个项目需要遵循一定规则才能用 hotpack 打包。

1. 所有 js 用 ES6 模块语法 编写
2. 每个 html 文件在同目录下对应一个同名的 js 文件

为了能快速开始，可以用新建命令快速建立项目

``` bash
#建立 vue 项目
hotpack init vue my-vue
cd my-vue
npm install
hotpack dev
```
默认会启动web服务和socket服务,socket服务用来支持热重载，如果不需要，可以不开启

``` bash
#不开启web服务器。 不开启web服务器 的时候，hotpack 不开启热重载
hotpack dev --no-server
#关闭热重载
hotpack dev --no-watch
```

默认build会用缓存提高效率，可以忽略缓存

``` bash
hotpack dev --no-cache
```