vim-skills
==========


## 技巧

### 用VIM查看编辑二进制文件

vim中二进制文件的编辑是先通过外部程序xxd来把文件dump成其二进制的文本形式，  
然后就可以按通常的编辑方式对文件进行编辑，编辑完成后再用xxd 转化为原来的形式即可。

可分成以下几步完成：

  * 首先以二进制方式编辑这个文件: vim -b datafile  
  * 现在用 xxd 把这个文件转换成十六进制: :%!xxd  
  * 最后，用下面的命令把它转换回来: :%!xxd -r  

> 只有十六进制部分的修改才会被采用。右边可显示文本部分的修改忽略不计。
> xxd是Linux的一个命令，vim可以通过”!”来调用外部命令，其功能就是进行十六进制的dump或者反之。










