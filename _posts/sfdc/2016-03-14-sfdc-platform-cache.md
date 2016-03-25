---
layout: post
title: Salesforce Platform Cache
category: Salesforce
tags: [Apex] 
keywords: 
description: 
---


Salesforce Winter 16 终于推出了 Platform Cache !

如果用的是Salesforce Developer Account, 要使用Platform Cache是需要申请的。
一般1个星期左右就能被激活。
如果是Sandbox, 就直接能用。

很多服务器端的语言都有服务器端的Session，或者用view state做客户端的。
 如Java, .Net， 而Salesforce Apex 以前是没有的，就连静态变量也做不了类似
 的事情（apex的static variables跟其它平台不同），Visualforce是有view state，但是限制在135k, 也不太好用。 所以数据一般存在数据库，或者custom settings.

要使用Apex的Cache,需要先配置一下。
<img src="/uploads/2016/apex_cache_setup.png" align="left" height="700" width="200" >
---

目前有两类Cache可用： 

* Session Cache - 跟Salesforce自己用的Session差不多，跟登录用户相关联。
                  用户的Session结束了，Cache也就被清空了。 
                  Cache的有效期可以定制
                  但是最长8小时。
* Org Cache     － 任何用户都可以访问这种缓存， 可以定制有效期。
                   文档里面没有提到最长的有效期。但是经过我的测试，还是有
                   有效期的，应该在24小时以内。

经过我的测试Session Cache是不支持Sites用户的。
Org Cache是可以的。


