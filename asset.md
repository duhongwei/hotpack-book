# 资源
spack 会分析 html 文件对应的 js 文件中引用的依赖，得到每个 html 文件的需要的资源，分类输出。

> 资源关系必须写在 js 文件中，资源关系是通过 es6 Modue 的 import xxx 表达的

## javascript 和 css

spack 只支持用 ES6 模块语法来导入 css，js。限制采用一种方案会让项目具有一致性，一致性可以提高可维护性。

``` js
//导入js模块
import a from './a.js'
import * from './a.js'
import {a，b as b1} from './a.js'
//导入css
import 'b.css'
```
>ES6 路径必须是相对路径 `../a.js ./a.css` 或绝对路径 `/a.js` 。不能这样写 `a.js`
>如果写成 `import vue` 会把vue看做是 node module模块
>只有.js 后缀名可以省略 `/a.js`,`/a`是等效的
>目录下的index.js可以略写为 `/` `/a/` ,`/a/index.js`是等效的

## node module
spack 可以导入通过npm 安装的模块。

``` js
//直接写 vue，被认为是node 模块
import Vue from 'vue'
``` 

>为了简洁高效，spack不会去 `node_module` 中分析哪个文件是要引用的文件，而是需要你在配置文件中指定 alias，告诉 spack 文件的路径，spack直接读到。

比如要引入 spritejs
``` js
module.exports = {
  alias: {
    spritejs: {
      path: "dist/spritejs.js"
    }
 }
}
```

## 其它资源

比如引用图片资源

- html中引用图片 `<img src='./image/logo.jpg'>`
- css中引用图片 `background:url(../image/logo.jpg)`
- js中引用图片 `imgObj.src='/image/logo.jpg`

为了简化正则匹配，书写路径的时候，需要遵循一定的定法。spack用的正则这样的

``` js
let reg = /(\.\.\/\.\.\/\.\.\/image|\.\.\/\.\.\/image|\.{0,2}\/image|\/pages)\/[-/\\0-9a-zA-Z_]+\.(jpg|jpeg|png|gif|webp|svg|eot|ttf|woff|woff2|etf|mp3|mp4|mpeg)/ig
```
可以看出，这个正则带有强烈的个性化色彩，对于我过去遇到的项目来说，足够用，我也愿意遵循一定的规则来写路径。这也是为什么我把 spack 定义为 “定制个性化打包工具的一个参考”。你可以参考 spack的设计来定制属于你的和你的团队的打包工具，可以任意加进你的和你的团队的风格，也可以针对特定项目。虽然非通用的工具不是那么酷，但是它开发成本低，而且又非常灵活。因为是为你的项目而量身定做的，所以会很舒适。 

## vue单文件

vue 单文件浏览器不能直接解析，需要借助 vue 插件。vue 插件已经内置。spack 检测到有 *.vue 文件的时候，会自动使用 vue 插件把单文件拆分成css,js文件, template会被编译并嵌入到js中，所以一个.vue文件，最后会生成两个文件，样式文件和js文件。

## HTML

Html文件是入口文件。html 只是 放 html 静态内容，不建议直接引用 js,css。和它同名的js文件负责逻辑和资源引入。这是一个规范。比如 `a.html`对应的 `js`文件 就是 `a.js`

``` html
<html>
<head>
 <!-- 不需要 引入 css. 编译后会自动插入到这里 -->
</head>
<body>
  <!-- 需要正常引入图像文件，文件路径编译后会自动替换成产品路径-->
  <img src="./images/header.png">
  <a href="./other.html">链接到其他页面</a>
  <!-- 不需要 引入 JavaScript,编译后会自动插入到这里 -->
</body>
</html>
```
也可以直接引用资源，但这样 spack就不会去做处理了。

## 动态引入资源
动态引用是做单面应用的必备，是绝对不能少的功能

``` js
fucntion load(){
  import('/node/vconsole.js').then(Vconsole => new Vconsole({
      onReady: function () {
      }
   }))
}

```