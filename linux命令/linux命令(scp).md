## (scp)命令笔记

`scp`是一个远程拷贝命令，`secure copy`的缩写。它的命令格式基本与`ssh`命令一致，只是在指定端口是用大写的`-P`。

下面命令是把当前目录下的1.py文件拷贝到远端家目录下的Desktop目录下。

注意:如果`:`后面不是绝对路径，则以家目录为参照。

> scp [-P port] 1.py root@remote:Desktop/1.py

下面命令是把远端目录Desktop下的1.py拷贝到当前目录下。

> scp [-P port] user@remote:Desktop/1.py 1.py

***

加上`-r`参数后可拷贝文件夹。

下面命令是把当前目录下123文件夹拷贝到远端的Desktop目录下。

> scp [-P port] -r  123 user@remote:Desktop/123

把远端的Desktop目录拷贝到当前目录下并命名为`demo`。

> scp [-P port] -r user@remote:Desktop/ demo

