# 转换
许多打包工具需要你安装和配置插件来转换资源，简单高效是 hotpack 追求的目标，一般情况下不需要手动安装插件就能满足大部分应用场景。 

所有 options 都可以写在 `plugin ` 配置项里。

## ES6 module
hotpack 要求所有 js 都是用 ES6 module 语法编写。hoptack 内置解析器，不需要配置。 

## buble 

作为代码转换器 babel 更有名。但是 babel 庞大而复杂，而 buble 高效而精悍，和 hotpack 的特质更加贴近，更好的是，buble 几乎不需要任何配置。可以在配置文件中配置是否需要buble,如果不需要去掉  ` buble:true ` 即可

``` js
plugins:{
	buble:true
}
```
> [buble的官方文档](https://buble.surge.sh/guide/)

正象官方文档所说，buble只负责转换语法，所以 polyfill需要自己搞定。幸运的是,hotpack 准备了一个插件，可以很方便的启用polyfill。有了buble,polyfill 这样就可以 ES5 环境用ES6语法了。

``` js
plugins:{
	buble:true,
	polyfill:true
}
```

可能您更偏爱 babel,没问题！hotpacK的所有功能都是插件来完成的，把buble换成babel，一句代码的事
``` js
plugins:{
	//buble:true,
	babel:true
}
```
不幸的是，现在还没有 babel 插件，邀请您来开发一个
## PostCss
[PostCSS](http://postcss.org/) 是一个使用插件转换 CSS 的工具。postcss 插件已经内置。

autoprefixer 是postcss的插件，需要在项目中安装才能使用。

``` js
const autoprefixer=require('autoprefixer')
module.exports = {
	"plugins": {
		//相关参数和写法参见 
		//https://github.com/postcss/postcss/blob/master/README-cn.md 
		//https://github.com/postcss/autoprefixer
		postcss:[autoprefixer] 
	}
}
```
>因为要用 PostCSS 就要安装插件，安装插件就会安装 PostCSS，所以 hotpack 并没有内置 PostCSS,需要的时候会到运行的项目中去查找。这样做会更安全，避免插件版本和 postCSS 版本不匹配的情况

## vue单文件

象 head.vue 这样的文件浏览器不能直接解析。为了能解析 head.vue 文件，需要借助 vue 插件。vue 插件负责把vue单文件拆分成css ，js。html作为模块嵌入在 js 中。

hotpack 会在需要的时候自动运行 vue 插件，不需要配置 
