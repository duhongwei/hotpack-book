# 唯一标识
所有的资源在`hotpack`中都是用统一的格式表示的，理解这一点非常重要。

## 规则
1. src 目录做为工作目录
2. 所有资源都相对于src目录
3. 格式形如 `a/b/c.js`
4. 不能省略后缀名,
5. node模块 `node/xx.js`
6. 第三方模块 `other/xxx.js`
7. .min.js结尾的js表示是压缩过的文件，不再会被解析和压缩

`src`的值默认就是`src`,在配置文件中可配置。唯一标识相当于是资源的身份证，用来唯一表示资源。

第三方模块就是不是通过 npm install的资源，是直接把文件copy过来放在项目中的 `src/other`下的资源，比如 jquery，我们可以直接把文件拿过来放在项目中
`src/other/jquery.js` jquery的key就是 `other/jquery.js`，如果想省略压缩的步骤，可以给文件重命名`src/other/jquery.min.js`，jquery的key变成`other/jquery.min.js`


## 举例

工作目录 `src`，在src下有文件 a.js,b.css，还有一个文件夹image,image文件夹有c.png。

a.js key值 'a.js'

b.css key值 'b.css'

c.png key值 'image/c.png'

## 哪里需要唯一标识

### group
默认把会所有js打包成一个文件，css打包成一个文件输出，这样对于多页应用来说不利于浏览器缓存。所以在配置中有group项，可以把资源打包成多个文件输出。group中的资源都用 key 来表示

```js
group:[
  [
    'js/common.js',
    'other/jquery.js'
  ],
  [
    'css/reset.css,
    'css/common.css
  ]
]
```
group是一个二维数组。里面的每个数组都会单独打包成一个文件。css,js不能混写。输出多个文件是对于多页项目的优化，是对整站而言的。所以即使某个页面只引用了 `js/common.js` ，在输出的时候也会包含`other/jquery.js`，因为他们在一个输出文件中，只要包含一个，就意味包含全体。

>虽然可以用算法自动生成group，但在实践中发现，还是手动维护更加灵活方便。

