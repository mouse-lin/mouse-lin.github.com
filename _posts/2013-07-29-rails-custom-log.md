---
layout: post
title: "rails custom logger"
description: ""
category: Ruby
tags: [Ruby]
---

## Rails Logger
---

**介绍**:

本文并不是介绍Rails Logger是怎样工作，仅仅从一个现实的例子来说明应该怎样使用Rails Logger

**目的**:

或许你会在一个rails app里面有许多不同apps，这个时候，你会想把它们的log在不同的log file上，而不是全部堆积在一起；又或许，你会想把里面一个单独功能log给分离出来

**How to**:

进入项目 config/environments/ ，分别在development.rb | staging.rb | production.rb配置

	config.logger = Logger.new("the_log_path_you_want_log") 

另外一种是指定文件与输入

	new_logger = Logger.new("#{Rails.root}/log/#{the_file_log_name}")

	new_logger.info "new log messages here"
	
好了，上面这些就搞定了log自定义
