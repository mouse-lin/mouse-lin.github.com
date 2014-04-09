---
layout: post
title: "ios multiple localizable"
description: ""
category: iOS
tags: [ios, object-c]
---

## Introduction

Since I add a localizable file to my project, I found this problem: the
first time I load my app, I see the localised string of my key in a
label, the second time I load the app, the key string in a label, the
next time I load the app, everything is fine again.

## Solution

the issue is that some vender plugins include a localizable.file again,
and that my project include the same name file, so when I launch the app
either of these two localizable string files were picked up.

So, u can rename your localizable file name, like MyString.string, and
make it localization. use NSLocalizedStringFromTable to localize it.

```
NSLocalizedStringFromTable(@"Key", @"MyString", nil)
```
