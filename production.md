执行 `hotpack build` 会设置  环境变量 `NODE_ENV=production`，因为很多工具都会根据它来判断环境

``` bash
hotpack build
```
这将关闭监听模式和热模块替换。还会开启压缩来减少输出包文件的大小。 JavaScript 用 uglify-es ，CSS 用 clean-css 

### 关闭压缩

``` bash
hotpack build --no-minify
```

### 关闭 web server

``` bash
hotpack build --no-server
```

### 关闭缓存

``` bash
hotpack build --no-cache
```

### 输出目录

``` js
module.exports={
//开发环境是 ./dev
destination:'./dist'
```
### 打包
打包，就是把多个文件合成一个文件一起输出。

位置在 `.hotpack/config/index.js`

虽然在生产环境中才会涉及打包,放在公共配置中是为了让开发环境也执行打包定义,不实际打包。因为执行打包定义可能会增加资源，所以开发环境执行打包定义是为了让开发环境加载的Js，css和生产环境是一样的。

默认打包把所有文件打成一个文件输出。
``` js
module.exports={
  "packages": [],    //打包定义
}
```
比如打包定义为

``` js
 plugins:[
	{
		name:"html",
		packages:[
			 ["common.css","layout.css"],
			 ["module2.css"]
		]
	}
```
目前的css列表为

``` js
['common.css','module1.css','module2.css','module3.css','page.css']
```
打包过程 ，结果初始化为 `result=[]`

找到第一个包 `["common.css","layout.css"]` ，在css列表中如果找到common.css或 layout.css之一就把第一个包作为拿过来。列表中找到` common.css` 所以把第一个包拿过来

``` js
result=[["common.css","layout.css"]]
``` 
找到第二个包 ` ["module2.css"]` ，在css列表中如果找到 modules2.css 就把第二个包放到结果里，列表中找到` module2.css` 所以把第二个包拿过来

``` js
result=[["common.css","layout.css"]，["module2.css"]]
``` 
包定义已经结果，把css列表中余下的样式，打成一个包，放到最后 

``` js
result=[["common.css","layout.css"]，["module2.css"]，['module1.css','module3.css','page.css']]
```
在result中有三个数组，在生产环境中每个数组会合并打成一个包输出，文件名是对内容进行哈希得到

现在另有一个页面，打包定义相同，但css列表不同

``` js
['layout.css','module1.css','module2.css','module3.css','page.css']
```

没有 `common.css` 但是因为有 `layout.css` 所以也会输出 打包定义中的第一个包
``` js
["common.css","layout.css"]
```
现在这两个页面输出的第一个css文件是同名的。可能只这两个页面看不出什么，但如果页面多了，就会享受到缓存的好处。因为打包定义中定义的是整个网站最可能访问到的资源，访问过一个页面后，后面的页面会大概率命中缓存。
