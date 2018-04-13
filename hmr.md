# 模块热替换（HMR)

热模块重载 (HMR) 在运行时自动更新浏览器中的模块优化开发体验，无需刷新整个页面。这意味着在您代码小幅更改时可以保留应用程序的状态。hotpack 的 HMR 实现支持开箱即用的 JavaScript 和 CSS 资源。

>HMR 构建在生产模式下时自动禁用。在不开启web 服务器的时候自动禁用

保存文件时，hotpack 将重新编译所更改的内容，并将包含新代码的更新发送到唯一正在运行的客户端。所有相关模块都会重新运行。

>仅支持唯一客户端热重载，也就是说，同一时间仅有一个页面可以热重载

## vue模块
对于vue模块，hotpack 会自动调用 [vue-hot-reload-api](https://github.com/vuejs/vue-hot-reload-api) 

当vue单文件被修改，hotpack 把vue　单文件拆分成 js 和 css，再单独判断js,css内容有没有变化，如果内容就变化就发到客户端，没变化不处理。

对于 vue 模块的热重载支持已经内置，不需要配置。

## 普通模块
和其它工具的 accept 和 dispose 不同， hotpack的处理函数是 load,unload，外加一个热重载标识 isHot。hotpack的处理逻辑是：当模块本身或依赖更新时，重新运行模块。 

``` js
if(this.isHot){
 	//当重载发生时
}
this.load=function(preservedData){
	//模块加载时 可以用 unload函数返回的 preservedData中的数据来恢复数据
	//也可以用 this.data 。preservedData 和 this.data 指向的是一个对象
});
this.unload=function(){
   //模块即将被替换时,把需要保存的状态 返回
	return {
    }
});

```

## 热重载举例

最后在浏览器中呈现的都是 AMD 模块，在首次运行模块的时候，会为模块指定一个拥有（调用）者 
``` js
var owner={isHot:false,data:{}}
```
运行 AMD的 factory

``` js
factory.call(owner,dependences)
```
owner 在整个生命周期都存在且唯一。

### 避免事件重复绑定和模块级变量状态操持 this.isHot + this.data

``` js
var count=0;
var that=this;
if(!that.isHot){
  button.addEventListener('click',function(){
	count++
	 //保存 count
    that.data.count=count
  });
}
else{
  //再次加载时恢复数据
  count=that.data.count
}
```

### 也可以用load,unload
``` js
var count=0
var that=this
function clickHandler(){
	count++
}
button.addEventListener('click',clickHandler)
this.unload=function(){
   //解绑事件
   button.removeEventListener('click,clickHandler)
   //把状态保存下来
   return { 
    count:count
  }
});
ths.load=function(preservedData){
   //恢复 a的值
   count=preservedData.count
}
```

这两种方式都是可以的。用 isHot有时更简洁， load,unload可以把热重载相关的集中处理。
