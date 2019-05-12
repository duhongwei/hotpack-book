# 快速开始
 
``` bash
npm install -g @duhongwei/spack
```
clone一个示例项目,然后用`spack`编译

``` bash
clone https://github.com/duhongwei/spack-vue-template
spack dev
```
`spack dev` 也可以写成 `spack`
默认以开发方式运行在 `3000` 端口，可以用 -p 指定其它端口 
``` bash
spack dev -p 5000
```

开发完成后发布 `spack pro` ，生成好的文件会放在 `dist` 目录

默认用缓存提高效率，可以忽略缓存 -c 同 --clearn

``` bash
#开发
spack dev -c
#发布
spack pro -c
```
发布的时候默认是不开启 server 的，可以用 -s开启

``` bash
#发布完成后时候开启server
spack pro -s
```

如果在开发的时候用 -s 是关闭 server的意思

``` bash
#发布完成后时候关闭server
spack dev -s
```
## 编译原理 ##

1. 读取项目中的文件（可以配置不想读取的文件）
2. 忽略没变化的文件
2. 对每个文件转码，压缩等，让所有文件都已经可以在浏览器中运行
3. 把Html文件作为入口文件，找到对应的同名 js文件，生成可以在浏览器中运行的html文件

html文件没有对应的js 文件会被认为是纯Html文件。
