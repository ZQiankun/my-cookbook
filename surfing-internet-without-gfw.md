## 科学上网

### Shadowsocks

1. **搭建VPS服务器**

2. **搭建Shadowsocks服务**

3. **客户端配置**

   **Linux平台**

   系统：Fedora 25

   * 安装shadowsocks相关应用

     ```shell
     $ sudo dnf copr enable librehat/shadowsocks
     $ sudo dnf update
     $ sudo dnf -y install m2crypto python-setuptools
     $ sudo easy_install pip
     $ sudo pip install shadowsocks
     $ sudo dnf install shadowsocks-qt5 # 此为图形化界面客户端，可不需要
     ```

   * 修改`/etc/shadowsocks.json`

     ```javascript
     # 如果该文件不存在，可以手动创建，并写入以下信息
     {
      "server":"xx.xx.xx.xx",  
      "server_port":xxx,
      "local_address": "127.0.0.1",
      "local_port":1080,
      "password":"xxxx",
      "timeout":600,
      "method":"aes-256-cfb",
     }
     ```
     server   服务端监听的地址
     server_port     服务器的端口（只要不与现有的端口冲突即可）
     local_address     本地监听的地址，直接写127.0.0.1
     local_port     本地的端口（不冲突即可，默认为1080）
     password     你的shadowsocks连接密码
     timeout     超时时间，单位秒
     method     加密方式。默认是: "aes-256-cfb"。

   * 启动ss 客户端

     ` sslocal -c /etc/shadowsocks.json`

     若要在开机时自启动，在 /etc/xdg/autostart/ 下创建desktop文件 shadowsocks-autostart.desktop即可。

     ```
     [Desktop Entry]
     Type=Application
     Name=SS Daemon
     Name[zh_CN]=SS 守护程序   
     Exec=sslocal -c /etc/shadowsocks.json -d start
     OnlyShowIn=GNOME;
     NoDisplay=true
     X-GNOME-Autostart-Notify=true
     X-GNOME-AutoRestart=true
     ```

   * 配置浏览器

     1. 安装google chrome浏览器

        ```
        google-chrome]
        name=google-chrome
        baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
        enabled=1
        gpgcheck=1
        gpgkey=https://dl.google.com/linux/linux_signing_key.pub
        ```

        将文件写入到`/etc/yum.repo.d/`下的文件中，然后`dnf update`，即可通过`dnf install pkgs` 的方式安装`google-chrome-stable`

     2. 安装代理扩展SwitchyOmega

        可以通过chrome的应用商店，也可以[在 Github 上下载最新版安装包](https://github.com/FelisCatus/SwitchyOmega/releases)。

     3.  SwitchyOmega 配合 [GFWList 项目](https://github.com/gfwlist/gfwlist)使用

        * 导入备份

          ![Step 1](https://github.com/FelisCatus/SwitchyOmega/wiki/images/t1/step1.png)

          按照下图，在导入导出页面点击“从备份文件恢复”,[备份文件参考](https://github.com/FelisCatus/SwitchyOmega/wiki/GFWList.bak)。

          如果曾经在SwitchySharp里配置过了，直接用SwitchySharp导出的备份文件也行的，不过要记得按照此页面最下方的说明，更改一下规则列表的网址。

          图中第三步会打开一个选择文件的对话框，这时候选择刚下载好的备份文件就行了。

          ![Step 2](https://github.com/FelisCatus/SwitchyOmega/wiki/images/t1/step2.png)

        * 设置代理服务器

          此处根据代理方式修改，如使用shadowsocks，则5中填写sock5，其他参考ss客户端的配置。

          ![Step 3](https://github.com/FelisCatus/SwitchyOmega/wiki/images/t1/step3.png)

        * 更新规则列表

           [GFWList 项目](https://github.com/gfwlist/gfwlist)是一个经常更新的项目，提供的规则列表也是需要定期下载更新的。

          ![Step 5](https://github.com/FelisCatus/SwitchyOmega/wiki/images/t1/step5.png)

          gfwlist新地址： `https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt`

     ​




