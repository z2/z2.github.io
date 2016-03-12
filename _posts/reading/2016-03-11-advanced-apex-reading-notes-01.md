---
layout: post
title: Advanced Apex 阅读笔记01
category: 读书
tags: [Apex] 
keywords: Salesforce, force.com, Apex
description: 
---
## Execution Context （EC）

这是要搞懂Apex非常重要的一个概念，也是Apex跟其他语言不大一样的地方。

    1. 定义了Static Variables的生命周期，这个跟其它server side languages 不同。
    2. 语言的限制（governor limits)（GL）跟这个相关，EC变换了，也就重置这些限制。
    3. 要注意的是不同的EC, 限制也不同。


能触发EC的情况:

	A database trigger;
	Future call;
	Queueable Apex;
	Scheduled Apex;
	Batch Apex;
	Web service;
	VisualForce and Lightning components;
	Global Apex;
	Anonymous Apex;

一般来说，如果Apex 触发了其它的Apex运行，EC会是同一个，
也就是说GL（governer limits) 没有重置，是同一组。

	EC starts => Trigger 1 => Trigger 2 => Trigger2 => EC ends


{% highlight java %}
if(!myclass.firstcall)
{
        // First call into trigger
        myclass.firstcall = true;
}
else
{
        // Subsequent call into trigger
}
{% endhighlight %}






