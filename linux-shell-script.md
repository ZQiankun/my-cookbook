# Linux Shell 脚本

----------
## 一些常用的使用技巧

1. sudo执行上一条命令

    `$ sudo !!`
    
    > 两个感叹号其实为bash的一个特性，称为事件引用符。`!!`相当于`!-1`,引用前一条命令，同理可以`!-2`，`!-100`等
    
2. 以Http方式共享当前文件夹的文件

    `$ python -m SimpleHTTPServer`
    
    > 这个命令启动了Python的SimpleHTTPServer模块，命令执行后将开启8000端口Http服务，可以在其他机器上通过 http://ip:8000 访问。

3. 普通用户打开vim保存root用户文件

    `:w ! sudo tee %`
    
    > 在vim中命令`:w!{cmd}可以让vim执行一个外部命令。%为vim中一个只读寄存器的名字，总保存着当前编辑文件的文件路径。`