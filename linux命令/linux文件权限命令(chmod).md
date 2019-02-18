## linux系统文件权限及chmod命令

#### linux查看文件权限

> ls -l

一共有10个字符，来表示文件的类型和权限:`drwxrwxrwx`  

(`d`表示`d`是文件夹，如果是`-`，则表示是非文件夹)

(`r`表示`read`，即读权限，`w`表示`write`，即写权限，`x`表示`execute`，即执行权限)

前三个`rwx`表示`user`用户组权限，中间三个`rwx`表示`group`用户组权限，最后三个`rwx`表示除上述用户组外的用户组权限。

***

#### 修改文件权限命令

> chmod u+r  t1.py 

上述命令表示user用户组对于t1.py文件增加可读权限。

`user`用户组简写`u`, `group`用户组简写`g`,其他用户组简写`o`，所有用户组简写`a`

多个用户组用时加上或减去权限

> chmod ug-r t1.py
>
> chmod ug-rw t1.py

***

#### 使用./的方式运行python文件

首先，我们需要在python文件最上边加上一句:

```python
#!/usr/bin/python3
```

告诉系统你的python3解释器的位置。

接下来使用`chmod`命令，为你的python文件增加可执行权限x。

这样我们就能使用`./1.py`的形式运行python文件了。