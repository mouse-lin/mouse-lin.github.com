---
layout: post
title: "JavaScript 实现"
modified: 2014-11-05 18:15:59 +0800
tags: [JavaScript]
image:
  feature: abstract-1.jpg
comments: true
share: true
---

### JavaScript 与 ECMAScript
----
通常 JavaScript 和 ECMAScript 被人们用来表达相同的含义，但 JavaScript 的含义却要比 ECMA-262 中规定的要多很多。

完整 JavaScript 实现应该由以下三个不同部分组成：

* 核心 (ECMAScript)
* 文档对象模型 (DOM)
* 浏览器对象模型 (BOM)

<img src="{{ site.url }}/images/JavaScript/js_ecma.png" alt="">

### ECMAScript
----

由 ECMA-262 定义的 ECMAScript 与 Web 浏览器没有依赖的关系。实际上，它并不包含输入与输出的定义。

ECMA-262 定义的只是这门语言的基础，并且在此基础上可以构建更完善的脚本语言。我们常见的 Web 浏览器只是 ECMAScript 实现的宿主环境之一。宿主环境不仅仅提供提供基本的 ECMAScript 实现，同时也提供了扩展的方式，以便于语言在与环境之间的对接交互。而这些扩展 - 如DOM，则利用 ECMAScript 的核心类型与语法提供更多更具体的功能，来实现针对环境的操作。

ECMAScript 的标准规定了 JavaScript 以下组成部分：

* 语法
* 类型
* 语句
* 关键字
* 保留字
* 操作符
* 对象

所以，ECMAScript 就是对实现以上标准规定的各个方面内容的语言描述。例如 JavsScript 实现了 ECMAScript，同样 Adobe ActionScript 也实现了 ECMAScript。
