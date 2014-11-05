---
layout: post
title: "微信分享网页缩略图、链接、标签"
modified: 2014-05-06 10:21:46 +0800
tags: [html]
image:
  feature: abstract-10.jpg
comments: true
share: true
---

现在微信越来越火了，很多HTML5 Page直接嵌入到微信的WebView里面打开，
都会涉及到这个页面被分享出去时，标题、图片、链接、介绍等设置。

微信分享比较主要有两种：

* 好友圈
* 好友

首先，我们先在页面里面用Javascript定义我们要被分享参数：
{% highlight javascript %}
var imgUrl = '图片url';
var lineLink = '分享后，点击跳转链接';
var descContent = "介绍";
var shareTitle = '标题';
var appid = 'wxc9937e3a66af6dc8'; //appid，也可以不带，带了的话底下会有对应认证应用图标
{% endhighlight %}

然后就是定义三个不同分享方法：

{% highlight javascript %}
function shareFriend() {
    WeixinJSBridge.invoke('sendAppMessage',{
                            "appid": appid,
                            "img_url": imgUrl,
                            "img_width": "640",
                            "img_height": "640",
                            "link": lineLink,
                            "desc": descContent,
                            "title": shareTitle
                            }, function(res) {
                            _report('send_msg', res.err_msg);
                            })
}
function shareTimeline() {
    WeixinJSBridge.invoke('shareTimeline',{
                            "img_url": imgUrl,
                            "img_width": "640",
                            "img_height": "640",
                            "link": lineLink,
                            "desc": descContent,
                            "title": shareTitle
                            }, function(res) {
                            _report('timeline', res.err_msg);
                            });
}
function shareWeibo() {
    WeixinJSBridge.invoke('shareWeibo',{
                            "content": descContent,
                            "url": lineLink,
                            }, function(res) {
                            _report('weibo', res.err_msg);
                            });
}
{% endhighlight %}

> 这里要注意是，android客户端，参数即使为空都必须带入，不然客户端会分享出不来

当微信内置浏览器完成内部初始化后会触发WeixinJSBridgeReady事件：

{% highlight javascript %}
document.addEventListener('WeixinJSBridgeReady', function onBridgeReady() {

        // 发送给好友
        WeixinJSBridge.on('menu:share:appmessage', function(argv){
            shareFriend();
            });

        // 分享到朋友圈
        WeixinJSBridge.on('menu:share:timeline', function(argv){
            shareTimeline();
            });

        // 分享到微博
        WeixinJSBridge.on('menu:share:weibo', function(argv){
            shareWeibo();
            });
}, false);
{% endhighlight %}
