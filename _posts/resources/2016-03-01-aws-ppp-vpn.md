---
layout: post
title: VPN
category: 程序资源
tags: [Salesforce VPN] 
keywords: 
description: 
---

在国内用Salesforce的朋友肯定觉得国内资源太少，
需要用Google才能查到资料，dreamforce的诸多录像都放在YouTube
上，所以要用好Salesforce，就自然而然的想到VPN。

方法很多，我喜欢用Amazon Web Service EC2自己假设PPTP VPN

1. 申请AWS（需要美金信用卡）.
2. 创建一个Ubuntu虚拟机.
3. 安装PPTP｀sudo apt-get install pptpd｀
4. 配置PPTP，并修改chap-secrets添加用户｀/etc/ppp/chap-secrets｀
5. 完成，可以配置客户端访问Google,Youtube.

详细的教程网上有很多。
`background-color`