
## 环境要求

因为`hotpack`是用 `type:'module'` 方式编写的，所以需要 node `>=node14`。

## 初体验

```bash
npm install -g hotpack
```
为了能快速体验，我精心准备了一个demo （demo还未完成）

```

clone vue 示例项目,然后用`hotpack`编译

``` bash
clone https://github.com/duhongwei/hotpack-vue-template
hotpack dev
```
`hotpack dev` 也可以写成 `hotpack`
默认以开发方式运行在 `3000` 端口，可以用 -p 指定其它端口 
``` bash
hotpack dev -p 5000
```

根据这个demo,介绍一下单页面，多页，服务端渲染，同构的功能


好了，如果你觉得`hotpack`是你想要的，让我详细介绍一下自己吧。