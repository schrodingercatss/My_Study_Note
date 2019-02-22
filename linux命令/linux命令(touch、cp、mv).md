## (touch、cp、mv)命令笔记

 #### 创建文件命令

> touch  [路径] + filename

**touch**命令：touch命令可以创建一个单文件，如果路径省略，那么就在当前目录下创建该文件。

若文件已存在，则会修改文件的末次修改日期。

***

#### 拷贝文件命令

> cp [路径] + old filename   [路径]  + new filename

**cp**命令：cp命令可以拷贝一个单文件，如果路径省略，默认在当前目录下进行操作，如果不想改变文件名，则只需要指定目标路径即可。

若要拷贝文件夹，则需要加入一个`-r`参数，表示递归拷贝。

> cp -r  [路径] + old dirname  [路径]  + new dirname

拷贝命令如果新文件/文件夹在目标路径下有重名，带上`-i`参数，则可以防止被覆盖。

> cp -i [路径] + old filename   [路径]  + new filename



***

#### 移动文件命令

> mv [路径] + filename   [路径] + filename

可以使用mv命令对一个文件 or 文件夹进行重命名。可以使用`-r`参数防止覆盖文件。

> mv [当前路径] + old filename  [当前路径]  + newfilename

***

