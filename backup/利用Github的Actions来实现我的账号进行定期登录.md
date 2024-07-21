前言
因serv00服务器要求，如果用户帐户在 90 天内未通过 DevilWEB 或 SSH 面板正确登录，则该帐户将自动从系统中删除，并且无法恢复该帐户收集的数据。

于是我就利用Github的Actions来实现我的账号进行定期登录，达到自动保号的效果！

有需要的朋友欢迎fork我的仓库：[传送门](https://github.com/bin862324915/serv00-automation)

以下是自动保号的操作流程

准备工作
一个GitHub账号。
Fork仓库。
准备好serv00账号。


配置Secrets
进入你fork本仓库后自己的仓库页面>Settings > Secrets中添加以下Secrets

SSH_INFO：包含SSH连接信息的JSON字符串。以下是示例

[
  {"hostname": "服务器号", "username": "用户名", "password": "密码"},
  {"hostname": "s5.serv00.com", "username": "user", "password": "password"},
  {"hostname": "s6.serv00.com", "username": "user6", "password": "password6"}
]
json
PUSHPLUS_TOKEN：用于推送通知的API令牌。以下是示例

QSchSXne1MzKA3sSuOTHP0dPOR2tEmHDjw
bash


测试运行
在GitHub仓库的Actions选项卡中，手动触发运行一次工作流程。
“Actions”页面>"Run SSH Login">"Run workflow">"Run workflow"
检查运行结果，没有报错说明就是运行成功了，可以点击运行记录的列表进去查看运行的详细情况


定时运行
此工作流默认是每七天的晚上9点运行一次

可以根据自己的需求调整运行时间

- cron: '0 11 */7 * *'
yaml
注意事项
保密性: Secrets 是敏感信息，请确保不要将它们泄露到公共代码库或未授权的人员。
更新和删除: 如果需要更新或删除 Secrets，可以通过仓库的 Secrets 页面进行管理。
通过以上步骤，你就可以成功将代码 fork 到你的仓库下并运行它了。如果需要进一步的帮助或有其他问题，请随时告知！

视频演示
YouTube视频演示地址：https://youtu.be/TNB9tcJi_Uo

B站视频演示地址：https://www.bilibili.com/video/BV1MNbseUERW/


-------------------------------------------------------------------------
