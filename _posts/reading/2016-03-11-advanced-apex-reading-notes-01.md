---
layout: post
title: Advanced Apex 札记01
category: 读书
tags: [Apex] 
keywords: Salesforce, force.com, Apex
description: Advanced Apex Programming for Salesforce
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

至于platform cache 参看我的博文[platform cache试用](http://blog.arkloud.com/2016/03/14/sfdc-platform-cache.html)

静态变量还有个不错的用途就是Caching Data, 减少SOQL的使用次数，提高程序性能。
我写了个小例子就是判断当前开发环境是不是沙盒：

{% highlight java %}
public class StaticVariableDemo {
	private static boolean isSandbox = false;
    private static boolean isSandboxCached = false;
    
    public static boolean isSandbox(){
        if(isSandboxCached){
            System.debug('isSandboxCached');
            return isSandbox;
        }
        
        Organization org = [select issandbox from organization];
       
        isSandboxCached = true;
        isSandbox = org.IsSandbox;
        System.debug('Read data from soql isSandbox: ' + isSandbox);
        return isSandbox;
    }
}
{% endhighlight %}

在developer console里面运行Anonymous code:

{% highlight java %}
StaticVariableDemo.isSandbox();
StaticVariableDemo.isSandbox();
StaticVariableDemo.isSandbox();
{% endhighlight %}

Log:

{% highlight text %}
17:39:01:014 USER_DEBUG [15]|DEBUG|Read data from soql isSandbox: false
17:39:01:014 USER_DEBUG [7]|DEBUG|isSandboxCached
17:39:01:014 USER_DEBUG [7]|DEBUG|isSandboxCached

{% endhighlight %}

Salesforce的EC里面也有内存限制，你不能任意读取很多数据放到缓存里面。
有些时候还是要从数据库里读取，要根据你实际情况，找到平衡点。

| Description      | Synchronous Limit | Asynchronous Limit  |
| ---------------- | ----------------- | ------------------- |
| Total heap size 4|              6 MB |               12 MB |



Salesforce其实是在教会你如何写出高效的代码，而在其它平台里面程序员往往不考虑
这些问题，而是乱写一气，只是表面上功能实现了。
多年前我在一个论坛里看到，有个网站说用户多了，就很慢，几乎不能用。
先不看数据库层面设计的合理不合理，看了他们的日志，
居然是每个用户每查看一页，菜单都是重新从数据库里提取，而这些菜单也都是静态的，不会变化，profiler日志里面满眼都是读取菜单。
如果菜单放到Cache里面，就是真正的做到多快好省。

静态变量还有一种设计模式就是在Trigger里面嵌套一个Class, 把Static variables 加在这个Class里面，这样就可以从外面赋值。而你无法从外面给Trigger
里的变量赋值。尤其是用在Test Methods里面。

## Limits

云计算是非常让人激动的技术革新，我们再也不用关心繁杂的硬件维护升级，软件版本的升级，
不用担心均衡负载，数据备份，数据库合理的搭建等等问题，这些问题就交给世界一等一的好手们来做。

资源总是有限的，我们不能无限制的使用。云计算也就是大家共用一个平台，彼此不能互相干扰。
我知道有些hosting companies, 他们的架构不算太好(如果选用的不是dedicated server的话)，
如果出现有用户进行大量的IO操作，就影响到其它用户的速度。就如同你自己开了一个杀毒软件在扫描磁盘，运行其它程序就觉得特慢。

Apex一般有两种限制，一种是针对一个execution context (EC)的，一种是针对一个Oranization 24小时的。

#### SOQL Limits

技巧：

* 尽量用 Bulk，
* 能用 before triggers 就不要用 after triggers, 
* 合理应用 Cache  
* Include fields from related objects in a single query

#### CPU Time Limits

``` text
Maximum CPU time on the Salesforce servers
同步	10,000 milliseconds	
异步	60,000 milliseconds

Maximum execution time for each Apex transaction
10 mins
```


#### DML Limits

技巧：
*  bulk DML
*  如果程序中多次用到DML操作，不要立即执行，尽量存储到List 或者 Map， 到最后再执行。

#### Heap Size

技巧：
* SOQL数据的时候，只是包含需要的字段，避免查询 long text or rich text fields.
* 根据情况，存储sObject Ids,到需要用的时候再查询，而不是把数据都放到static variables.

Apex 里面还有一个隐含的用法，一般用户不知道。就是如果查询很多记录的时候。

``` java
//如果有1万条Account记录，下面的 List 就存储了1万条记录在内存里。

List<Account> accounts = [select id,name from account];
for(Account a : accounts){
	//do sth.
}

//如果采用下面的方法，SOQL同样算是一条查询,不增加占用，
//但是每次返回200条记录。

for(List<Account> accounts : [select id,name from Account]){
	for(Account a : accounts){
		//do sth.
	}
}

//如果处理的问题更简单，采用简写
for(Account a : [select id,name from account]){
	//do sth here.
}

```

#### Describes 

用来获取Force.com的 MetaData. Force.com cached 这些数据，不再算做限制，
这是考虑到Schema.describeSObjects()可能会消耗大量 CPU time. 用起来小心。

#### Callout



＝＝> to be continued

Charles.Chen@arkloud.com

References:
* [Advanced Apex Programming(Dan Appleman)](http://advancedapex.com/)
* [Developer Force](http://developer.force.com)