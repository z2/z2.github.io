---
layout: post
title: Advanced Apex 阅读笔记01
category: 读书
tags: [Apex] 
keywords: Salesforce, force.com, Apex
description: 
---
## Execution Context （EC）

EC是Apex语言里非常重要的一个概念，也是Apex跟其他语言不大一样的地方。

    1. 定义了Static Variables的生命周期，这个跟其它server side languages 不同。
    2. 语言的限制（governor limits)（GL）跟这个相关，EC变换了，也就重置这些限制。
    3. 要注意的是不同的EC, 限制GL也不同。


能触发EC的情况:
{% highlight raw %}
	A database trigger;
	Future call;
	Queueable Apex;
	Scheduled Apex;
	Batch Apex;
	Web service;
	VisualForce and Lightning components;
	Global Apex;
	Anonymous Apex;
{% endhighlight %}

一般来说，如果Apex 触发了其它的Apex运行，EC会是同一个，
也就是说GL（governer limits) 没有重置，是同一组。

	EC starts => Trigger 1 => Trigger 2 => Trigger2 => EC ends


 如果Triggers 跟workflow updates 交织在一起就不好玩儿了,
 下面就是解决的方法。

{% highlight java %}

public Static Boolean firstcall = false;


if(!myclass.firstcall)
{
        // First call into trigger
        myclass.firstcall = true;
}
else
{
        // Subsequent call into trigger
}

// Excerpt From: Dan Appleman. “Advanced Apex Programming for Salesforce.com and Force.com.”
{% endhighlight %}


### Static Variables

大部分计算机语言的静态变量一般都是在应用层面的（application level),
往往用于共享类之间的数据，或者给一些类纪录生成实例的数量等等。
而Apex却不一样！ 

> Static variables in Apex have execution context scope and lifetime.

在Winter'16之前，我们是没有办法用Apex存贮数据到Session，
我们也不能用Static Varivales在不同的transaction之间来存取数据，
只能采用数据库，或者custom setting 来做， 问题就是数据库访问比较慢也占用
Soql的limits,而custom setting不占用soql,不如session灵活，数据容量的限制也要注意。
{% highlight text %}
The total amount of cached data allowed for your organization is the lesser of these two values:
* 10 MB
* 1 MB multiplied by the number of full-featured user licenses in your organization
{% endhighlight %}

至于｀platform cache 参看我的博文[platform cache](sfdc/2016-03-14-sfdc-platform-cache.md)


