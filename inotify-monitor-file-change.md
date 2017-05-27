> inotify 是一种文件变化通知机制，linux 内核从2.6.13开始引入。本文主要说明在 linux 下如何使用 inotify 实时监控文件变更。

1. **如何知道使用的 linux 内核是否支持 inotify 机制**

   `grep INOTIFY_USER /boot/config-$(uname -r)`，若 CONFIG_INOTIFY_USER = y，则说明内核已经支持inotify机制。

2.  **shell中inotify的使用**

   + 安装inotify-tools，提供inotifywait命令；

   + 修改user监控的最大文件数

     ```
     /proc/sys/inotify/max_user_watches   默认为8192 
     修改为 echo 81920000 > /proc/sys/inotify/max_user_watches
     ```

   + 将文件变化列表导入到文件中

     ```
     inotifywait -rm -e create -e delete -e modify  /dir1  /dir2 ... -o changefile.list
     ```

     通过 `-e`设置监控事件，包括访问、创建、修改等