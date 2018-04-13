# 命令行接口(Command Line Interface)

全局安装好 hotpack 后，执行 hotpack -h 发现有五个命令

Commands:

``` shell
list           list available official templates
init           generate a new project from the template
build          production compile
dev            dev compile
help [cmd]     display help for [cmd]
```
### hotpack list
其中 list 是 hotpack 的 default 命令 当执行 `hotpack` 的时候，就是在执行

	hotpack list

输出显示有两个模板
	
	Available official templates:
		★  react - The project template which you can go with react
		★  vue   - The project template which you can go with vuemodule
 
### hotpack init
hotpack init 是新建项目，执行`hotpack init -h`

	Usage: hotpack-init <template-name> <project-name>

	Options:
    	-h, --help  output usage information
	Examples:

    	# create a new vue project named apple
	    $ hotpack init vue apple

hotpack init 需要两个参数，一个是模板名，可以是 vue 或 react，一个是项目名称。
### hotpack dev

	Usage: hotpack-dev [options]
	Options:
		--no-cache   ignore file version,rebuild all files
		--no-watch   no watch file and hotload
		--no-server  no server service
		-h, --help   output usage information

### hotpack build
	Usage: hotpack-build [options]
	Options:
		--no-cache   ignore file version,rebuild all files
		--no-server  no server service
		--no-minify  no minify
		-h, --help   output usage information
  