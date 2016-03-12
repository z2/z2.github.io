---
layout: post
title: Saleforce 开发心得
category: 读书
tags: [Apex] 
keywords: Salesforce, force.com, Apex
description: 
---

> Salesforce有自己的语言做开发，在2015年 [stackoverflow.com](http://stackoverflow.com/research/developer-survey-2015#tech-super)做的调查中
Salesforce 的开发语言被评为Most Dreaded, 排在第二的是 Visual Basic.
Swift则被评为Most Loved.

我从08年开始接触Salesforce, 开始的时候肯定是不喜欢的，当时也没有什么书籍，官方论坛里面冷冷清清，
只能自己看文档。由于时间原因不可能看完文档，融会贯通后再开发，
所以遇到的坑就多，而且这些坑是在其它开发环境里面遇不到的。 印象最深的是我写的第一个for loop 就出现了内存超出限制（[salesforce governor limits](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm)），
搞得一头雾水，这才知道原来force.com Apex 语言有限制，
而且这样的限制有很多很多，强烈感觉被束缚了，被绑架了。

我想这也是同行们不喜欢的原因吧。

但是随着时间的推移，在其它平台遇到的问题越多，尤其是面对复杂框架出现的
种种性能问题越多，才越来越能体会出Apex的种种限制设计的精妙。 这些设计绝对是一顶一的高手们的杰作。
一位优秀的程序员的程序就不该超出这些限制，
超出了往往就说明你采用的方法是有问题的，需用寻找更好的方法来实现。


在这里我强烈建议一本[Salesforce Apex Programming for Salesforce.com and Force.com](http://advancedapex.com/)
这本书刚出了第三版，Dan Appleman的作品。
这本书是讲述了如何用这种独特的语言思考，而是不是如何使用这种语言。
想成为好手靠的是技能，而想成为高手靠的是思想，形而上的东西。












