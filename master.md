# 完全控制
简单是 hotpack 的目标之一，所以默认情况下，hotpack 会把复杂的处理逻辑都屏蔽掉，让使用变得简单。但是如果想随心欲，还是不得不做深入的了解。

## 高级配置
用 `init` 命令生成的项目中的配置文件只展示了一部分配置。这部分是容易修改的部分。还有一部分没有开放出来，因为这部分配置比较复杂或是不常用。虽然没有开放出来，但是还是可以正常修改的。
``` js
// config/index.js 公共配置没有展示的默认配置 
{
    //所有插件都可以访问的数据
    "metadata": [],
    //是否要清空 destionation 目录
    "clean": false,
    //build or process,区别是 process 不会输出文件到 destionation 目录
    "method": 'build',
	//打包定义，
    "packages": []
}
// config/dev.js 开发环境没有展示的默认配置
{
    "before": [
      { 'ensureString': true },
      { 'node': true },
      { "vue": true, },
      { 'runtime': true },
      { 'slim': true },

    ],
    "after": [
     { "parseJs": true },
      { 'html': true }
    ]
}
// config/pro.js 开发环境没有展示的默认配置
{
"before": [
      { 'ensureString': true },
      { 'slim': "image" },
      { 'upload': "image" },
      { 'replaceImage': true },
      { 'node': true },
      { 'vue': true },
      { 'runtime': true },
      { 'slim': true },
    ],
"after": [
      { 'parseJs': true },
      { 'compress': true },
      { 'upload': true },
      { 'html': true }
    ]
}
```
打包定义详见 [生产环境](production.md) 的打包一节

高级配置里最重要的也是最复杂的就是 before 和 after 。

### 最终插件列表
前面说过，hotpack中所有功能都由插件完成。为了方便使用，把常用的和复杂（顺序很重要）的插件都放在 before ,after中，不希望（但没有限制）用户修改。

最终的插件列表是这样得来的
``` js
plugins=before.concat(plugins).concat(after)
```
为什么说有些插件的执行顺序很重要呢？因为hotpack的执行是沿一个方向进行的，而且不可逆。也没有事件机制可以改变执行路线。所以就需要合理的顺序来实现缓存等功能。


----------


有果有问题可以 [提issue](https://github.com/duhongwei/hotpack/issues)