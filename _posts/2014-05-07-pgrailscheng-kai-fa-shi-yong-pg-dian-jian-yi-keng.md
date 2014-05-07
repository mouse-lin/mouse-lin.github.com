---
layout: post
title: "Rails程开发使用PG一点建议(坑)"
modified: 2014-05-07 15:20:12 +0800
tags: [rails, ruby, pg]
image:
  feature: abstract-3.jpg
  credit: 
  creditlink: 
comments: true
share: true
---

* ### NULLS LAST

开发时候，我们往往需要对一个表某些字段进行排序，这些字段难免存在为null的值，例如：

{% highlight ruby %}
@answers = Answer.order("course_id DESC")
{% endhighlight %}

注意上面代码，在Mysql里面返回顺序是这样的，存在course_id排在前面，course_id为null排在后面，在PG却相反，所以这个时候需要这样来写：

{% highlight  ruby %}
@ansers = Answer.order("course_id DESC NULLS LAST")
{% endhighlight %}

* ### Order column 必须被select

还是同样排序问题，排序中我们也经常会joins另外表，对于另外表得字段进行排序，例如：

{% highlight ruby %}
@answers = Answer.joins(:broadcast_records).order("broadcast_records.id desc")
{% endhighlight %}

上面代码无论是在Mysql还是PG都不会出错的，但要是加上uniq或者select("DISTINCT")，例如：

{% highlight ruby %}
@answers = Answer.joins(:broadcast_records).order("broadcast_records.id desc").uniq
{% endhighlight %}

{% highlight ruby %}
@answers = Answer.joins(:broadcast_records).order("broadcast_records.id desc").select("DISTINCT answers.*")
{% endhighlight %}

这时就会抛出以下的错误（恭喜你，你又可能导致一个线上bug出现了）：

{% highlight ruby %}
PG::InvalidColumnReference: ERROR:  for SELECT DISTINCT, ORDER BY expressions must appear in select list
{% endhighlight %}

根据上面错误，就是排序字段必须在select list里面，所以我们可以这样来更改：

{% highlight ruby %}
@answers = Answer.joins(:broadcast_records).order("broadcast_records.id desc").uniq.select("answers.*, broadcast_records.id")
{% endhighlight %}
