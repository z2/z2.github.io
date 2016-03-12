---
layout: post
title: Advanced Apex 阅读笔记01
category: 读书
tags: [Apex] 
keywords: Salesforce, force.com, Apex
description: 
---
## Context


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






