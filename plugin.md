# 插件

Hotpack 采用与许多其它工具稍微不同的策略，许多常见的格式都被开箱即用地包含进来，而不需要安装或者配置额外的插件。然而，有些情况你可能会想在非标准的情况下扩展 Hotpack 的能力，而那些时候，插件是被支持的。

## 编写插件

hotpack 使用 [metalsmith](https://github.com/segmentio/metalsmith) 作为基础组件，插件格式和 metasmith 插件是完全一样的。 每个插件都会接收到 三个参数

- files 从source目录读到的所有文件
- metalsmith 实例对象
- done 处理完成后的callback文件


1. 写插件的时候需要注意，一个插件只完成一件事情
2. 当插件处理过程有异步处理时，需要调用 done，来通知完成。

>插件参数不可以是function,因为当参数是 fuction 时候，会把它直接当做插件本身执行

``` js
//插件参数 opts 如果不需要可以省略
module.exports =function(opts) 
	reutrn function (files,metasmith,done) {
  	for(const file in files){
		if('file不是要处理的类型'){
			continue;
		}
		let contens=files[file].contents
		//对文件内容做处理
        files[file].contents=contents
	}
	//可选，如果都是同步，可以省略，但同时上面第一行中 的 done 也要去掉
	done()
};
```
## 发布插件
如果是通用插件，发布这个包到 npm, 并使用 hotpack-plugin- 前缀。

如果不是通用插件，可以直接放在项目中，只要不放在源文件目录(./src)即可

## 使用插件
在使用上分 内置插件 和 非内置插件

如果是内置插件，直接引用即可，比如 buble 就是一个内置插件

``` js
plugins:{
 buble:true
}
```

非内置插件需要 require 插件,得到插件对象

``` js
const myPlugin=require('实际路径')
module.exports={
 plugins:{
   // 当hotpack 判断出 myPlugin 的参数是一个函数，就知道这就是插件本身，不用再寻找了
   // myPlugin() 返回插件函数，见上面的插件例子
   myPlugin:myPlugin('这里可以写参数')
 }
}
```

## 配置
有两种形式，对象形式和数组形式
```  js
//对象形式，插件不能重复
plugins:{
  //无参数
  myplugin1:true
  //有参数
  myplugin2:[1,2,3]
  //有参数
  myplugin3:{image:'a.jpg'}
}
//数组形式，插件可以重复
plugins:[
  //无参数
  {myplugin1:true}
  //有参数
  {myplugin1:[1,2,3]}
  //有参数
  {myplugin1:{image:'a.jpg'}}
]
```

无论是对象形式还是数组形式，插件都会按引用的顺序执行
