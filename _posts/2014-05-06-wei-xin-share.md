---
layout: post
title: "微信分享网页缩略图、链接、标签"
modified: 2014-05-06 10:21:46 +0800
tags: [html]
image:
  feature: abstract-10.jpg
  credit: 
  creditlink: 
comments: true
share: true
---

* 代码
	
	其中要注意就是对于android手机客户端，参数即使为空都必须带入，不然客户端分享会出不来

{% highlight javascript %}
var imgUrl = 'http://qqfood.tc.qq.com/meishio/16/4585bf7c-be04-420f-ac8a-2dba61a7561f/0';
var lineLink = 'http://life.qq.com/weixin/r/lottery/13826036970196242008#wechat_redirect';
var descContent = "万达狂欢节, 夺宝幸运星大抽奖活动开始啦！";
var shareTitle = '万达狂欢节';
var appid = 'wxc9937e3a66af6dc8';

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
// 当微信内置浏览器完成内部初始化后会触发WeixinJSBridgeReady事件。
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
