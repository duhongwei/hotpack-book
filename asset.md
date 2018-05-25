# 资源
hotpack 会分析 html 文件对应的 js 文件中引用的依赖，得到每个 html 文件的需要的资源，分类输出。

> 资源关系必须写在 js 文件中，资源关系是通过 es6 Modue 的 import xxx 表达的

## javascript 和 css

hotpack 只支持用 ES6 模块语法来导入 css，js。限制采用一种方案会让项目具有一致性，一致性可以提高可维护性。

``` js
//导入js模块
import a from './a.js'
import * from './a.js'
import {a，b as b1} from './a.js'
//导入css
import 'b.css'
```
>ES6 路径必须是相对路径 `../a.js ./a.css` 或绝对路径 `/a.js` 。不能这样写 `a.js`
>如果写成 `import vue` 不带后缀名的形式， 会把vue看做是 node module模块 ，到node_modules目录去查找对应的 ES6格式的文件。详见下文的 node module 中的说明

## node module
hotpack 可以导入通过npm 安装的模块。

``` js
//不带后缀名，也不带路径，被认为是node 模块
import Vue from 'vue'
``` 

>为了简洁高效，需要在配置中直接指定相对于`node_module`的文件名

比如要引入 vue 的 node 模块
``` js
module.exports = {
"node": {
    "vue": "vue/dist/vue.esm.js"
 }
}
```

## 其它资源

不需要声明依赖。正常使用即可，和平时写页面一样。

比如引用图片资源

- html中引用图片 `<img src='logo.jpg'>`
- css中引用图片 `background:url(../logo.jpg)`
- js中引用图片 `imgObj.src='/logo.jpg`

路径符合 html 路径规则即可。

## vue单文件

vue 单文件浏览器不能直接解析，需要借助 vue 插件。vue 插件已经内置。hotpack 检测到有 *.vue 文件的时候，会自动使用 vue 插件把单文件拆分成css,js文件。html 会做为 template 嵌入到js中

## HTML

Html文件 是 http 请求 的入口文件。不过在开发阶段，html 只是 放 html 静态内容，不引用 js,css。和它同名的js文件负责逻辑和资源引入。这算是一个规范。

``` html
<html>
<head>
 <!-- 不需要 引入 css. 编译后会自动插入到这里 -->
<link type="text/css" rel="stylesheet" href="style.css"/>
</head>
<body>
  <!-- 需要正常引入图像文件，文件路径编译后会自动替换成产品路径-->
  <img src="./images/header.png">
  <a href="./other.html">链接到其他页面</a>
  <!-- 不需要 引入 JavaScript,编译后会自动插入到这里 -->
  <script src="./index.js"></script>
</body>
</html>
```
## 动态引入资源

比如 动态引用 user.js

``` js
fucntion load(){
  let user=()=>import('./user.js')
}
```