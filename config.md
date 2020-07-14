# 配置文件
要想运行`hotpack` 需要先准备配置文件。

`hotpack`曾经的方案是可以不用准备配置文件，可以使用默认的配置。但在 `1.0`中取消了默认配置。

1. 如果有默认配置，就需要了解默认的配置都是什么，否则可能结果非预期。
2. 项目的配置文件不包含所有的配置，让配置文件不够明晰，不能表达所有信息。

总之，取消默认配置的原因是为了简化配置。现在把所有的指挥权都交到你这了。

## 简介

配置的文件夹必须在项目的根目录，并以 `.hotpack`命名。在文件夹下有三个文件,base.js,dev.js,pro.js，分别对应基础配置，开发配置和发布配置。

在开发环境，`hoptack`读取 base.js,dev.js，命令行的设置并merge得到最后的配置，如果有相同的后面的会覆盖前面的。

在生产环境，`hoptack`读取 base.js,pro.js，命令行的设置并merge得到最后的配置，如果有相同的后面的会覆盖前面的。

命令行的配置优先级最高,其次是 dev,js,pro.js，最后是 base.js

## 详细配置

下面列表所有的配置
```js
//压缩插件
import compress from '@duhongwei/hotpack-plugin-compress'

export default{
  //源文件目录，src里面的文件会全部读取并进行处理
  src: 'src',
  //输出目录。
  dist:'dist'
  //分组。在生产环境，为了让浏览器缓存多页面的公共js,css。
  group: [
    [
      'other/jquery.js',
    ]
  ],
  //代理，koa，proxy插件实现。可以用来解决跨域的问题，也可以mock数据
  //配置详情 https://github.com/edorivai/koa-proxy
  proxy: {
    host: 'http://xxx.xxx', 
    match: /\/webapi\//,
    map: function (path) { return path.replace('/webapi', ''); }
  },
  //插件列表
  plugin:[
   { 
    //插件名称，日志输出会用这个name来标识是哪个插件
    name: 'compress',
    //插件
    use:compress,
    //参数
    opt: {
      name: 'afterRead'
    }
   }
  ]
}
```

`hotpack`只实现流程控制，所有的功能都由插件实现，更多的配置也都由插件负责。