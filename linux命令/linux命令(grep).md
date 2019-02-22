## (grep)命令笔记

#### 基本用法

grep可以对某一个文本文件的所有符合条件的内容进行高亮显示，用法如下。

> grep 查找的文本内容  查找的文件
>
> grep print 1.py

注：如果查找内容是一个带空格的词或句，则需要使用引号把他们引起来。

> grep "hello world" 1.py

***

#### 基本参数

`-n`显示行号及高亮关键词。

> grep -n print 1.py

`-v`，所有不包含关键词的行,相当于取反

> grep -v print 1.py

`-i`，忽略大小写查找关键字。

> grep -i print 1.py

***

#### 模式查找（正则查找）

`^a`：行首，表示以a开头的行

> grep ^print 1.py

`ke$`:行尾，表示以ke结尾的行

> grep print$ 1.py











