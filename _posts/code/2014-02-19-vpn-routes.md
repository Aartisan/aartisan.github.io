---
layout: post
title: VPN Routes设置 让国内网站速度快点
description: "转载文章"
date: 2014-02-19
category: code
---

1. 购买VPN，设置好VPN相关配置
2. 下载 chnroutes.py，相关地址https://code.google.com/p/chnroutes/downloads/list
3. 下载 chnroutes.py 文件之后进入终端，找到文件执行相关命令 `python chnroutes.py -p mac`，命令执行后会在目录下生成「ip-up」，「ip-down」两个文件
4. 拷贝以上两个文件到 /etc/ppp 目录，执行 `sudo chmod a+x ip-up ip-down`

完成操作，终端输入 `netstat -nr`检查路由表的输出信息，连接VPN之后再次执行，你会发现路由表变化了。
