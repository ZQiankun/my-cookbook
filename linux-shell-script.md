# Linux Shell 脚本

## cron 的使用详解

> 定时任务被用于安排那些需要周期性执行的命令。cron 是 linux 最为实用的工具之一，cron 服务在后台运行，并会持续性地检查 /etc/crontab 文件及 /etc/cron.*/ 目录。

1. **crontab 命令**

  crontab 是用来安装、卸载或列出定时任务列表的命令。因为每个用户都可以拥有自己的 crontab 文件，虽然这些文件都位于 /var/spool/cron/ 目录中，但并不意味着你可以直接编辑他们，所以 crontab 命令就很有必要！

2. **cron 有两个配置文件**，一个是全局配置文件（/etc/crontab），是针对系统任务：一组是 crontab 命令生成的配置文件（/var/spool/cron目录下的文件），是针对于某个用户的定时任务配置。

3. **crontab 任务配置基本格式**

    ```
        *          *           *            *          *
    分钟(0-59)  小时(0-23)  日期(1-31)  月份(1-12)  一周当中的某天(0-6)  command
                                                    [7或者0表示周日]

    ```
​                                                                                                                             

​	操作符允许为一个字段指定多个值。

​	星号（*）：此操作符为字段指定所有可用值，如在小时字段下使用则表示每个小时，同理在月份字段下表示		

​	每个月；

​	逗号（,）：指定了包含多个值的列表，如：1,5,10,20;

​	横杠（-）：指定一个范围，如5-15等同于5,6,7 ... 15;

​	正斜杠（/）：用来指定一个步进值，为一间隔频率，如 0-59/2 用来在分钟字段定义每两分钟；如果希望每2	

​	小时执行一次，则在小时字段下 \*/2

4. **其他**

   默认情况某个命令或者脚本的输出会发送到本地邮箱，若想停止接收则在执行命令 command 后加入 `> /dev/null 2>&1 ` ，也可以通过 /etc/crontab 中的MAILTO修改邮箱账户

   * crontab 	-e  编辑任务；
   * crontab   -l  列出当前用户下的任务；
   * crontab   -r  删除当前所有定时任务；
   * crontab   -u usename  -l  （需要root权限）

5. **特殊字符**

   @reboot    @yearly（@annually）    @monthly     @weekly     @daily      @midnight      @hourly

   以上可以替他crontab配置基本格式的前五个字段，如@hourly command

   |        目录         |                    描述                    |
   | :---------------: | :--------------------------------------: |
   |   /etc/cron.d/    | 类似于/etc/crontab总的配置，不过/etc/cron.d中添加的配置需要重启crond服务才能生效，而/etc/crontab中更新的配置则不需要重启服务。corn.d中的配置文件权限必须为644，且属主为root |
   | /etc/cron.daily/  |              运行时需要每天执行一次的脚本              |
   | /etc/cron.hourly/ |             运行时需要每小时执行一次的脚本              |



## at 命令用法

> at 命令用来安排一个程序在未来进行一次执行。提交的任务都会被放在 /var/spool/at 目录下，并且到了执行时间时由 atd 守护进程来执行。
>
> 使用该服务前需要确认 atd.service 是否开启

1. **命令格式：**

   at \[参数\]\[时间\]

2. **时间格式**

   * HH:MM              ex: 04:00
   * HH:MM YYYY-MM-DD          ex：04:00 2017-05-20
   * HH:MM \[am\|pm\]\[Month\]\[Date\]          ex：04am March 17
   * HH:MM \[am\|pm\] + number \[minutes\|hours\|days\|weeks\]    ex； now+5minutes  or  04pm+2days

3. **at的运行方式**

   at命令产生的计划任务以文档的形式写入 /var/spool/at 目录内，该任务等待 atd 服务取用与运行，因安全需常通过 /etc/at.allow 与 /etc/at.deny 这两个文件来对 at 进行限制。

   若 /etc/at.allow 存在，则写入文件中的使用者才可以使用；

   若 /etc/at.allow 不存在，则寻找 at.deny 文件，写入其中的使用者不能使用 at ；

   两者皆不存在，仅root可使用at

4. **实例参考**

   * 一天后的下午5点执行

     ```shell
     at 5pm + 1days
     ```

   * 明天17点钟执行

     ```shell
     at 17:00 tommorrow
     ```

   * 可以使用 atq 来查看系统执行任务列表

   * atrm 对任务进行删除

     `atrm job-number`

   * at -c job-number 对任务进行查看



## 一些常用的使用技巧

1. sudo 执行上一条命令

    `$ sudo !!`

    > 两个感叹号其实为bash的一个特性，称为事件引用符。`!!`相当于`!-1`,引用前一条命令，同理可以`!-2`，`!-100`等

2. 以Http方式共享当前文件夹的文件

    `$ python -m SimpleHTTPServer`

    > 这个命令启动了Python的SimpleHTTPServer模块，命令执行后将开启8000端口Http服务，可以在其他机器上通过 http://ip:8000 访问。

3. 普通用户打开vim保存root用户文件

    `:w ! sudo tee %`

    > 在vim中命令`:w!{cmd}可以让vim执行一个外部命令。%为vim中一个只读寄存器的名字，总保存着当前编辑文件的文件路径。`

4. 切回上一个目录

    `$ cd -`

5. 替换上一条命令中的一个短语

    `$ ^foo^bar`

    > 相当于`!!:s/foo/bar/`

6. 快速备份一个文件

    `$ cp filename{,bk}`

7. 免密码ssh登录主机

    ```
    $ ssh-keygen
    $ ssh-copy-id remote-machine
    ```

    > 这个命令将当前用户的公钥串写入远程主机的~/.ssh/authorized_keys内，这样下次登录就不用输入密码。

8. 抓取linux桌面视频

    `ffmpeg -f x11grab -s 1366x768 -r 25 -i :0.0 -qscale 0 /tmp/out.mpg`

    > 由于ffmpeg暂未加入Fedora的官方源中，普通用户可以到[ffmpeg官网](/www.ffmpeg.org)下载编译安装，或者将rpmfusion加入到仓库源列表中，以获取ffmpeg相关文件。

    > ffmpeg通常用法是根据一些参数输出一个文件，输出文件一般置于最后。
    > -f x11grab    指定输入类型
    > -s xxxx    设置抓取区域大小，通常可以使用xrander查看当前系统屏幕大小
    > -r 25    设置帧率
    > -i :0.0    设置输入源，本地X默认为0.0
    > -qscale 0    保存更输入流一样的像素质量    

9. 清空或者创建一个文件

    `> filename`

    > "\>"在shell中是标准输出重定向符，即把（前部）命令行输出转往一个文件内，由于“前部命令”为空，输出为空，于是就覆盖（或创建）成一个空文件了。

10. 重置终端

  `reset`

   > 一般cat了一个二进制文件会造成终端显示大量乱码，没法回显，此时可以通过该命令进行恢复。

11. 在午夜时执行命令

    `echo cmd | at midnight`

    > 主要是at的使用，可以参考at命名详解

12. 映设一个内存目录

    `mount -t tmpfs -o size-1024m tmpfs /mnt/ram`

13. 用diff对比远程文件和本地文件

    ```
    ssh user@host cat /path/to/remotefile | diff /path/to/localfile -
    ```

14. 查看系统中占用端口的进程

    `netstat -tulnp`

    > netstat是常用的用来查看linux网络系统的工具之一，其中-t显示tcp链接信息；-u显示udp链接信息；-l显示监听状态端口；-n直接显示ip；-p显示相应的进程PID。

15. 更友好地显示当前挂载的文件系统

      `mount | column -t`

       > column 用于把输出结果进行列表格式化操作。 

16. 实时查看某个目录下最新改动的文件

    `watch -d -n 1 'df; ls -FlAt /path'`

17. 执行一条命令但不保存到history中

    空格 + command

18. 显示当前目录中所有子目录的大小

    `du -h --max-depth=1`

19. 显示消耗内存最多的10个运行中的进程

    `ps aux | soet -nk +4 | tail`

20. 查看ASCII码表

    `man 7 ascii`

    > 通过 `man man`可以查询很多有用的信息及使用说明

21. 简易计时器

    `time command`

22. 在一个shell中运行命令

    （command） 或者 \`command\`

23. 清空屏幕

    CTRL + l  或者 tput clear

    > tput sc 存储光标位置
    > tput rc 恢复光标位置
    > tput ed 清除当前位置到行尾的内容

24. 快速地知道服务器重启完成

    `ping -a ip`

    > 此为ping命令Audible ping参数的一个用法，在ping通后会有声音输出。

25. 查询ANSI color对应值

    `dircolors -p`

    > 这个颜色值可以用于设置字符及背景颜色，如 `echo -e "\e[1;31m XXXX\e[0m"`

26. 输入密码时不显示内容

    ```
    #!/bin/sh
    echo -e "Enter password:"
    stty -echo
    read password
    stty echo
    ```

27. 去除文件中的重复行

    `awk '!a[$0]++'`

28. 列出最常用的10条命令

    `history | awk '{a[$2]++}END{for(i in a){print a[i] "_" i}}' | sort -rn | head`














