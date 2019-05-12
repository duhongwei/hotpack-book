# 转换
许多打包工具需要你安装和配置插件来转换资源，简单高效是 spack 追求的目标。

## ES6 module
spack 要求所有 js 都是用 ES6 module 语法编写。hoptack 内置解析器，不需要配置。 

## buble 

作为代码转换器 babel 更有名。但是 babel 庞大而复杂，而 buble 高效而精悍，和 spack 的特质更加贴近，更好的是，buble 几乎不需要任何配置。可以在配置文件中配置是否需要buble,如果不需要去掉  ` buble:true ` 即可

``` js
plugins:{
	buble:true
}
```
> [buble的官方文档](https://buble.surge.sh/guide/)

正象官方文档所说，buble只负责转换语法，所以 polyfill需要自己搞定。幸运的是,spack 默认会引用polyfill，所以不用任何处理就可以放心编写es6代码

可能您更偏爱 babel,没问题！spacK的所有功能都是插件来完成的。
``` js
plugins:{
	//buble:true,
	babel:true
}
```
不幸的是，现在还没有 babel 插件，您可以开发一个
> buble不支持 for of 语法 和 awawit。
> 
## PostCss
[PostCSS](http://postcss.org/) 是一个使用插件转换 CSS 的工具。postcss 插件已经内置。

autoprefixer 是postcss的插件已内置

## vue单文件
已内置
 
