# 联结

在一个项目中有很多文件，html,css,js，如何知道，哪些文件构成一个页面？我们需要一个js文件负责把整个页面的资源联结到一起。虽然`hotpack`不要求这个文件的命名和位置，但建议把一个页面的资源都放在一个文件夹中，其中的index.js负责联结页面资源。对于公共的资源可以单独设文件夹

假设有这一个文件夹 about ,包含三个文件，index.js,index.css,about.html,about.js，还有一个image文件夹

联结都是在 index.js 中完成。约定：所有的联结都在 index.js中完成

## 联结html
```js
//联结html模板
import './index.html'
```
这句的意思是 `index.js`依赖`index.html`。也可以理解成 `index.html` 的入口是`index.js`
```js
//联结html模板,指定发布后的web路径 /2020/about/index.html
import './index.html=>2020/about/index.html'
```
默认情况下`index.html`的发布路径为 `/about/index.html` 通过 `=>`可以改变发布路径，

```js
//联结html模板,指定发布后的web路径 /2020/about/index.html
import './index.html=>2020/about/index.html'
//联结html模板,指定发布后的web路径 /3030/index.html
import './index.html=>3030/index.html'
```
可以多次联结`html`，这样做的结果是一个html文件被发布成多个文件。这个功能很少用到，但是一但用到了，就会特别方便。

## 联结 css

```js
//联结 ./index.css
import './index.css
```
这句的意思是 `index.js`依赖`./index.css`。结合前面联结`html`模板，现在输出逻辑是：`index.html`的入口是 `index.js`，`index.js`导入了 `index.css`，所以目前 `index.html`拥有了 `index.css`

## 联结 js

``` js
//导入js模块
import a from './a.js'
import * from './a.js'
import {a，b as b1} from './a.js'
```
按 ES6 模块语法导入 js 模块

>ES6 路径必须是相对路径 `../a.js ./a.css` 或绝对路径 `/a.js` 。不能这样写 `a.js`
>如果写成 `import vue` 会把`vue`看做是 `node module`模块
>只有.js 后缀名可以省略 `/a.js`,`/a`是等效的
>目录下的index.js可以略写为 `/` `/a/` `/a`,`/a/index.js`是等效的
>`/a`可能代表`/a.js`，也可能代表`/a/indexjs`


## 联结图片

- html中引用图片 `<img src='./image/logo.jpg'>`
- css中引用图片 `background:url(../image/logo.jpg)`
- js中引用图片 `import logo from './image/logo.png' logoObj.src=logo`

在html文件，css文件中，同目录可以省略 “./”，比如在开头提过的`about`文件夹中，引用图片可以这样引用`<img src='image/logo.jpg'>`，`background:url(image/logo.jpg)` 但是在`js`文件不可以省略 “./”。这样规定是为了和浏览器的实际行为相符。


## 异步联结资源
动态引用是做单面应用的必备。用动态`import`语法即可

``` js
fucntion load(){
  import('/node/vconsole.js').then(Vconsole => new Vconsole({
      onReady: function () {
      }
   }))
}

```
## node module
因为不是核心功能，所以由功能由插件完成。