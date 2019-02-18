## (rm、rmdir)命令笔记

#### 删除文件/文件夹命令

> rm [路径] + filename
>
> rm -r [路径] + dirname
>
> rmdir [路径]  + dirname

rm要删除文件夹需要带上参数`-r`。

这里的`rm - r`  和 `rmdir`的区别就是，`rmdir`只允许删除空文件夹，而`rm -r`可以删除非空文件夹。

若要防止误删除，则可以带上参数`-i` (interactive),带上后，系统会询问你是否要删除该文件。

还有一个参数是`-I`,它和`-i`的区别就是，当文件超过3个时,`-I`才会询问你是否要删除，否则不会提示。

>  rm - i  [路径]  + filename
>
>  rm - I  [路径]  + filename