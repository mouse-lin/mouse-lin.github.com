---
layout: post
title: "如何正确获取Excel表里时间数据"
modified: 2014-06-26 15:02:47 +0800
tags: [rails,excel,ruby]
image:
  feature: abstract-3.jpg
  credit: 
  creditlink: 
comments: true
share: true
---

### Rails 对 Excel文件导入时间格式处理

* 问题

在用Ruby对Excel文档导入功能开发的时候，遇到过这样一个问题，导入数据创建的记录里面所有时间字段不是为了nil就是都是错误的时间。

只有在Mac使用Office Excel把时间重新编辑，然后保存一下后，才有正确时间数据保存下来，后来通过Log才发现，在windows Excel下面保存的时间都是Float类型，翻了下资料后才知道，Excel保存时间就是通过Float类型的，然后根据时间从1899,12,30 再加上这个Float天数算出来最终时间。

可以用这样公式计算出真正时间：

{% highlight ruby %}
  DateTime.new(1899,12,30) + excel_float_val.days
{% endhighlight %}

* 例子

  * 使用 [Roo](https://github.com/Empact/roo) Gem 来对Excel等文档进行读取

  * Excel导入实例代码

  {% highlight ruby %}
  def import_csv
    name = params[:file].original_filename                    #赋值上传文件名
    directory = "public/files"                                #赋值临时上传文件路径
    path = File.join(directory, name)      
    File.open(path, "wb") { |f| f.write(params[:file].read) } #打开文件
    xls = Roo::Excel.new(path)                                #生成Roo对象
    all_dates_arry = []
    xls.each_with_index do |line, index|                      #假定line[1]为日期行
      #先判断是否为Float，为Float类型，利用上面公式计算日期，否则直接返回
      date = line[1].is_a?(Float)? (DateTime.new(1899,12,30) + line[1].days) : line[1]
      all_dates_arry << date
    end
    all_dates_arry
  end
  {% endhighlight %}
