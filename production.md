执行 `hotpack pro` 会设置  环境变量 `NODE_ENV=production`，因为很多工具都会根据它来判断环境

``` bash
spack pro
```
发布的时候默认是不启动server的
###  web server
编译完成后开启server
``` bash
hotpack pro -s
```

### 清除缓存

一般情况下不需要清除缓存，因为一旦清除，所有文件必须重新编译，速度会大大降低。

``` bash
hotpack spack -c
```

### 输出目录

``` js
{
//开发环境是 dev
dist:'dist'
```
### publicPath ###
publicPath的意义是在一个web根目录下发布多版本的产品。

``` js
module.exports = {
  publicPath: '2020'
}
```
所有的生成的文件都会发布到 2020目录，web根目录也会变成 `/2020/`

### 打包
打包，就是把多个文件合成一个文件一起输出。

默认打包把所有文件分成一组打成一个文件输出。
``` js
{
  "group": [],    //默认打包定义
}
```
为了利用浏览器的缓存机制，可以把资源打成多个包

比如想把 css 打成两个包

``` js
{
 group:[
			 ["common.css","layout.css"],
			 ["module2.css"]
		]
}

```
css列表为

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