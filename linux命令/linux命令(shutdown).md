## (shutdown)命令

`shutdown`是关机命令，基本用法如下：

> shutdown [参数] [时间]

如果不指定参数和时间，则默认一分钟后关机。

`-r`参数，表示重启命令。

`now`表示当前时间。

下述命令表示立即重启。

> shutdown -r now 

下述命令表示系统将在`20:25`重启。

> shutdown -r 20:25

下述命令表示系统在10分钟后关机。

> shutdown +10

下述命令表示取消关机命令。

> shutdown -c

