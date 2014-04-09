---
layout: post
title: "Sql copy table datas from other table"
description: "把数据从一个表迁移到另外一个表"
category: Sql
tags: [Mysql, DB]
---

### 数据迁移

今天需要对用户表进行整理，把几十万Web用户从user表里面迁移出来放到另外一个表。

### Sql操作

注意字段是相同的

    insert into table column1, column2 select column1 column2 from table2 where query
