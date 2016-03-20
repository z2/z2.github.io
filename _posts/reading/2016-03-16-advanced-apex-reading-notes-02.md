---
layout: post
title: Advanced Apex 札记02
category: 读书
tags: [Apex] 
keywords: Salesforce, force.com, Apex
description: Advanced Apex Programming for Salesforce
---
## Bulk Patterns

Bulk几乎贯穿了整个force.com的开发，Apex Trgger, DML, SOQL, API, etc.
无处不再。 Bulk看上去挺简单的，但在平日见到的程序中，尤其是在其它平台上的各式程序中，
应用了这个思想的还真的不多。
为什么原来写一段代码就能搞定的事情，非要搞得如此复杂，不停地绕弯儿。

而且很多Force.com的Limits都是有意迫使你使用Bulk Patterns, 因为对Salesforce来说，带来的好处不是一点儿半点儿。
具体好在哪里，估计要写一本书才能说清楚。
简单说就是提升 perfomance and scalability.

说到这里就顺便提一下微信的API(其它部分代码也不得而知）,
居然没有Bulk的设计思想在里面。 


>	“All of your Apex code should be designed to handle bulk operations.
>    All of it – no exceptions.”

>   Excerpt From: Dan Appleman. “Advanced Apex Programming for Salesforce.com  and 
>  Force.com.”

---
Charles.Chen@arkloud.com

References:

* [Advanced Apex Programming(Dan Appleman)](http://advancedapex.com/)
* [Developer Force](http://developer.force.com)