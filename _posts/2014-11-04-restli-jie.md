---
layout: post
title: "REST 理解"
modified: 2014-11-04 11:16:49 +0800
tags: [http]
image:
  feature: abstract-3.jpg
comments: true
share: true
---

### 起源
---

REST (Representational State Transfer) **表现层状态转换**，是Roy Fielding在2000年的论文中提出。


### 定义
---

<u>RESTFUL</u> 在我读大学时期起，就经常接触到的一个软件架构风格，也是较为多人讨论的一个话题。
至于它具体的基本定义，可以看看维基百科上面 [REST](https://zh.wikipedia.org/wiki/REST "REST")。
**要注意的是REST是一套设计的风格，而非标准**。


### 资源 RESOURCE
---

RESOURCE表示网络上面的资源，在网络上面提供各种各样的资源，而这些资源我们可以通过直观的URI看出。例如URI是http://mouseshi.com，在浏览器输入它就可以直接来到我的Blog首页，而后，客户端在从菜单进入所有Posts分类的页面，此时URI为http://mouseshi.com/posts，一个URI代表一种资源，客户端需要获取不同资源，通过不同的URI来获取，这就是资源(RESOURCE)。

### 表现层 REPRESENTATION
---

上面提到资源，是一种信息的实体，它具有不同的外在表现形式（比如：TXT，JSON，XML，YAML等）。

把资源外在表现形式呈现出来的形式，就是表现层（REPRESENTATION）。

### 状态转换 STATE TRANSFER
---

在客户端与浏览器交互过程中，会涉及到数据一些操作，客户端通过Http协议，这里面有对资源不同操作方式，例如：GET、POST、PUT、DELETE，它们分别对应获取资源、创建新资源、更新资源、删除资源。

### REST 优点
---

* 让原本无状态的Http通过可以通过请求处理不同操作
* 浏览器即为客户端，简化软件需求
* 无需其他资源发现机制
* 在软件技术演进中的长期的兼容性更好
