# 命令行接口(Command Line Interface)

全局安装好 spack 后，执行 spack -h

Commands:

``` shell
Usage: spack <command> [options]

Options:
  -V, --version  output the version number
  -h, --help     output usage information

Commands:
  pro            production compile
  dev            dev compile
  help [cmd]     display help for [cmd]

```
### spack dev
执行`spack dev -h`
``` shell
Usage: spack-dev [options]

Options:
  -c,--clean            ignore file version,rebuild all files
  -d,--directory        work directory
  -p,--port [port]      web server port
  -s --server           server without
  -f,--folder [folder]  config folder
  -h, --help            output usage information

```
### spack pro
``` shell
Usage: spack pro [options]

Options:
  -c,--clean            ignore file version,rebuild all files
  -d,--directory        work directory
  -p,--port [port]      web server port
  -s,--server           web server
  -f,--folder [folder]  config folder
  -h, --help            output usage information
```
  