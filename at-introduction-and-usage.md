## at 命令用法

> at 命令用来安排一个程序在未来进行一次执行。提交的任务都会被放在 /var/spool/at 目录下，并且到了执行时间时由 atd 守护进程来执行。
>
> 使用该服务前需要确认 atd.service 是否开启

1. **命令格式：**

   at \[参数\]\[时间\]

2. **时间格式**

   ```
   HH:MM              04:00
   HH:MM YYYY-MM-DD      04:00 2017-05-20
   HH:MM \[am\|pm\]\[Month\]\[Date\]     04am March 17
   HH:MM \[am\|pm\] + number \[minutes\|hours\|days\|weeks\]    now+5minutes  or  04pm+2days
   ```

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

