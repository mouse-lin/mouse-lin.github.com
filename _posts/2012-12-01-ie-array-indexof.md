---
layout: post
title: "IE Array indexOf"
description: ""
category: Web 
tags: [Html, IE]
---

## IE Array no indexOf function

今天在开发写Js时候才发现，使用了indexOf地方，在IE浏览器下面都挂了，查了下，才发现，原来是IE下面Array没有indexOf这个方法。

**Solve way**:

    function(obj, start){
      for (var i = (start || 0), j = this.length; i < j; i++) {
        if (this[i] === obj)
          return i;
      }
      return -1;
    }
