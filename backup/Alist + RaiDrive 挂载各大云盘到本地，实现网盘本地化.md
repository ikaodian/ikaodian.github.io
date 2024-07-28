Alist是一个强大的云盘管理工具，支持多种云盘服务商，包括阿里网盘、百度云盘、Google Drive、OneDrive等，提供了文件同步、文件分享等丰富功能，可以将各大云盘整合到一个界面，方便用户管理和操作。

RaiDrive是一个用于挂载云盘到本地的工具，通过将云盘映射为本地磁盘，使得用户可以直接在本地文件管理器中访问云盘文件。RaiDrive支持多种云存储服务，包括但不限于Google Drive、Dropbox、OneDrive等，支持Windows和macOS系统。

NSSM，全称Non-Sucking Service Manager，是一个在Windows平台上运行的服务管理工具。它的设计目标是提供一种简便的方式来将任意可执行文件转换为Windows服务。通过NSSM，用户可以方便地将自己的应用程序以服务的形式在后台运行，而无需自己编写复杂的服务程序。

为什么选择使用Alist和RaiDrive挂载云盘？

Alist提供了统一的管理界面，方便用户在一个应用中管理多个云盘账户。

RaiDrive支持在本地文件系统中以驱动器的形式挂载云盘，使得访问云盘文件就像访问本地文件一样便捷。

nssm用来管理alist以服务的形式在后台运行

准备工作
alist下载地址：https://github.com/alist-org/alist/releases

alist文档：https://alist.nn.ci/zh/guide/

RaiDrive下载地址：https://www.raidrive.com/download

nssm下载地址：https://nssm.cc/download

nssm文档：https://nssm.cc/commands

nssm使用说明：https://wizops.net/archives/202312/221.html

配置 Alist
设置密码

下载alist，根据你的系统选择，window系统下载alist-window-amd64.zip即可


下载后解压到你指定的目录，进入目录按住shift+鼠标右键选择”在终端中打开”

在命令行中设置密码

# 手动设置一个密码 `NEW_PASSWORD`是指你需要设置的密码
.\alist.exe admin set NEW_PASSWORD
启动服务
# 运行程序
.\alist.exe server

然后在浏览器中打开：127.0.0.1:5244

使用用户名：admin和刚才设置的密码登录即可

挂载云盘
这里以阿里云盘为例演示添加网盘

登录后依次点击下列按扭来添加网盘

正下方的管理》左侧的存储》添加》选择“阿里云盘Open”


依次配置以下三项，刷新令牌在正文有说明，配置完成保存即可


获取刷新令牌

打开alist文档找到下方这里，点击刷新令牌链接，登录自己的阿里云盘即可获取


配置完成

这里状态为：work即为配置成功，回到主页就可以查看内容了，其它网盘参考alist文档配置。


配置 RaiDrive

RaiDrive下载后直接双击安装即可，依次按下图配置， 完成点击连接即可。



配置成功我的电脑就出现在Z盘，以后只需要在alist中添加网盘，都会添加到Z盘里


配置开机自启动

配置完成，但是启动alist的命令行窗口不能关，每次开机还需要手机打开，我们通过nssm把alist注册为window服务，在后台运行。

nssm的配置参考文档：nssm创建window服务

以管理员身份打开命令行窗口， 输入命令 nssm install alist  填写完成后点击Install service


再输入命令  nssm start alist  启动服务

C:\Windows\System32>nssm start alist
alist: START: 操作成功完成。
C:\Windows\System32>
文章来源：
https://www.bilibili.com/read/cv28686383/