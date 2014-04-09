---
layout: post
title: "Refactoring Ruby edition - 1"
description: "使用集合闭包方法替换循环"
category: Ruby
tags: [Ruby, Refactor]
---

### 动机

在大多数主流的语言里，操作集合都是通过循环来进行的，每次抓取一个元素然后进行处理。在Ruby中，出现这样情况，不妨考虑使用闭包来替换循环操作。

### 手法

 * (find)找出循环的基本模式 
 * (replace)把循环替换成适当的集合闭包方法
 * (test)测试

### 例子

在大多数常见情况下，有好集中有用的集合闭包方法，接下去，我来列出比较常见一种，刚开始时候可能会比较不习惯：

	managers = []
	employees.each do |e|
	  managers << e if e.manager?
	end

好的，接下去，我们可以替换成一下这种方式：

	managers = employees.select{|e| e.manager?}
	
reject 方式会逆置过滤器测试。在这两种情况下，除非你使用破坏性的形式(select! or reject!)，否则它们不会修改原来的集合。

	
	managerOffices = []
	employees.each do |e|
	  managerOffices << e.office if e.manager?
	end
	
换成这样方式：

	managerOffices = employees.select{|e| e.manager?}.collect{|e| e.office}
