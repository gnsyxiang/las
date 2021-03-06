nm-study
========


## 功能

列出.o .a .so中的符号信息，包括诸如符号的值，符号类型及符号名称等。所谓符号，通常指定义出的函数，全局变量等等。

```
nm [-A|-o|--print-file-name] [-a|--debug-syms]
   [-B|--format=bsd] [-C|--demangle[=style]]
   [-D|--dynamic] [-fformat|--format=format]
   [-g|--extern-only] [-h|--help]
   [-l|--line-numbers] [-n|-v|--numeric-sort]
   [-P|--portability] [-p|--no-sort]
   [-r|--reverse-sort] [-S|--print-size]
   [-s|--print-armap] [-t radix|--radix=radix]
   [-u|--undefined-only] [-V|--version]
   [-X 32_64] [--defined-only] [--no-demangle]
   [--plugin name] [--size-sort] [--special-syms]
   [--synthetic] [--target=bfdname]
   [objfile...]
```

## 使用

### 常用选项

* -A 在每个符号信息的前面打印所在对象文件名称；

* -C 输出demangle过了的符号名称；

* -D 打印动态符号；

* -l 使用对象文件中的调试信息打印出所在源文件及行号；

* -n 按照地址/符号值来排序；

* -u 打印出那些未定义的符号；

### 常见的符号类型

* A 该符号的值在今后的链接中将不再改变；

* B 该符号放在BSS段中，通常是那些未初始化的全局变量；

* D 该符号放在普通的数据段中，通常是那些已经初始化的全局变量；

* T 该符号放在代码段中，通常是那些全局非静态函数；

* U 该符号未定义过，需要自其他对象文件中链接进来；

* W 未明确指定的弱链接符号；同链接的其他对象文件中有它的定义就用上，否则就用一个系统特别指定的默认值。

### 注意事项

* -C 总是适用于c++编译出来的对象文件。
     还记得c++中有重载么？为了区分重载函数，c++编译器会将函数返回值/参数等信息附加到函数名称中去形成一个mangle过的符号，
	 那用这个选项列出符号的时候，做一个逆操作，输出那些原始的、我们可理解的符号名称。

* 使用`-l` 时，必须保证你的对象文件中带有符号调式信息，这一般要求你在编译的时候指定一个`-g`选项。

* 使用nm前，最好先用`file`查看对象文件所属处理器架构，然后再用相应交叉版本的nm工具。


### 举例

```
nm -A -l ./lib/* 2>/dev/null | ag "T wav_file_clean"
```






