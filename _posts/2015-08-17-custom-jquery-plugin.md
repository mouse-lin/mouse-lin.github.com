---
layout: post
title: "定制我们自己的 JQuery 插件"
modified: 2015-08-17 18:24:19 +0800
tags: [JavaScript JQuery]
image:
  feature: abstract-9.jpg
  background: witewall_3.png
comments: true
share: true
---

### 介绍
---

一直来使用过的 JQuery 插件很多，也写过一些 JQuery 库的扩展。

然而要真正了解插件如何扩展 JQuery 库需要对 JavaScript prototype 属性有一些基本的了解。

虽然说不直接使用，但是 JavaScript prototype 属性可通过 jQuery 属性 fn 在后台使用，这是原生 JavaScript prototype 属性的一个 jQuery 别名。


### JQuery 自动补全插件(JQuery AutoComplete Plugin)
---

* 第一步，定义一个名为 auto


