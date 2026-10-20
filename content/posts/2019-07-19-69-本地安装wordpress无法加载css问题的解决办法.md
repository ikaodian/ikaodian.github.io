---
title: 本地安装wordpress无法加载css问题的解决办法
date: 2019-07-19
categories: [aihao]
category_slugs: [aihao]
type: post
original_id: 69
original_url: https://kaodian.org/?id=69
---
本地安装wordpress之后，有时会出现CSS无法加载的问题，wordpress只显示文字，没有页面样式，包括wordpress后台也这样。有时候是在局域网中访问时，只显示文字，没有样式。  
出现这种情况是因为wordpress安装时用[http://localhost](http://localhost/)访问来安装，局域网访问时，加载css的路径不对，所以会出现css无法加载，只显示文字。  
  
解决wordpress无法加载css的问题也很简单。先忍受wordpress无法加载css的问题，进入wordpress后台，进入设置=>常规，设置“wordpress地址（URL）”和“站点地址（URL）”把这两个地址设置成本机IP就可以了。  
也可以直接进入mysql修改，比如打开phpmyadmin，找到WordPress数据中的wp_options字段，修改siteurl和home两条数据的值为“<http://192.168.1.102/wordpress>”。  
或者打开mysql console登录WordPress数据库。  
mysql>updatewp_optionssetoption_value='<http://192.168.1.102/wordpress'where>  
option_name='siteurl';  
QueryOK,1rowaffected(0.00sec)  
Rowsmatched:1Changed:1Warnings:0  
mysql>updatewp_optionssetoption_value='<http://192.168.1.102/wordpress'where>  
option_name='home';  
QueryOK,1rowaffected(0.00sec)  
Rowsmatched:1Changed:1Warnings:0  
以上三种方法，任选一种就解决问题了。  
有时候服务器上安装的wordpress启用第三方CDN的时候，刚开始也会出现CSS无法加载的情况，出现这种情况的解决办法是等一会儿，等CDN把文件都同步完了之后，CSS加载就正常了。

**本文地址：**<https://www.kaodian.org/?id=69>  
**版权声明：** 本文为原创文章，版权归 [fengzhikuang](https://www.kaodian.org/?auth=1) 所有，欢迎分享本文，转载请保留出处！ 

[PREVIOUS:涨知识！这些耳熟能详的成语，你知道来源哪些诗句吗](https://www.kaodian.org/?id=68)

[NEXT:get_option()函数常用表名称](https://www.kaodian.org/?id=70)

## 文章导航

[__](https://www.kaodian.org/?id=68)

[__](https://www.kaodian.org/?id=70)
