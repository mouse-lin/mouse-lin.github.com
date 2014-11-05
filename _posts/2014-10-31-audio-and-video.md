---
layout: post
title: "HTML5 audio & video"
modified: 2014-10-31 14:56:37 +0800
tags: [html5]
image:
  feature: abstract-3.jpg
comments: true
share: true
---


### 容器(container)与编解码器(codec)

<img src="{{ site.url }}/images/videos/video-container.png"
width="300px" class="pull-right">

*容器与编解码器是HTML5 audio与video两个比较重要的概念。*

不论音频文件还是视频文件，其实从本质上面来说，都是一个容器文件，例如压缩文件Zip。

从右边图我们可以看得出，视频容器包含了**音频轨道**、**视频轨道**和其他一些**元数据**。

视频播放的时候，音频轨道和视频轨道是绑定在一起的，元数据部分包含了该视频的封面、标题、
子标题、字母等相关信息。

#### 主流的视频容器支持如下视频格式

* Audio Video Interleave(.avi)
* Flash Video(.flv)
* MPEG 4(.mp4)
* Matroska(.mkv)
* Ogg(.ogv)

#### 音频和视频编解码器

音频和视频的编码/解码器是一组算法，用来对一段特定音频或视频流进行解码和编码，
以便音频和视频可以正常得播放。

对于原始的媒体文件来说，体积是非常大，假如不对其编码，那么构成一段视频和音频的数据将会
非常庞大，以至于在网络上面传播已经播放都需要等待很长时间。

音频编解码器

* AAC
* MPEG-3
* Ogg Vorbis

视频编解码器

* H.264
* VP8
* Ogg Theora

### Audio 和 Video 的优缺点

在页面中播放视频比较典型方式是使用Flash、QuickTime或者Window
Media插件，向HTML中嵌入视频，但是相对于这种方法，使用HTML5的媒体标签会有以下两点好处：

* 作为浏览器原生支持的功能，audio与video无线安装。虽然有一些插件安装率很高，但是在控制比较严格
的公司往往会屏蔽掉这些插件，或者是当插件被禁用后，也会出现无法播放的情况。

* 媒体元素向Web页面提供了**通用**、**集成**和**可脚本化控制的API**。对于开发人员来说，这个无疑是最大优点。

当然除了优点，也有些功能是HTML5规范中不支持的，例如：

* 流式音频与视频。因为目前的HTML5规范中还没有比特率的切换标准，所以对于视频的支持只限于
加载全部媒体文件。

  > 也存在第三方自己实现了整套包括音视频的采集、编解码、网络传输、显示等功能。例如Google
  的[WebRTC](http://baike.baidu.com/view/5855785.htm?fr=aladdin)(*Real-Time Communications*)，
  用它可以实现流式视频的播放。

* HTML5的媒体资源受到HTTP跨源(*cross-origin*)资源共享的限制。

* 全屏视频无法通过脚本控制。从安全性考虑，让脚本控制全屏操作的确不大合适。

* 对audio和video的访问未完全加入规范。

* 缺少通用编解码器支持。这点算是目前来说最大的缺点，只能随着时间推移，最后会有一套通用的编解码器。

### Audio 和 Video 的API

在使用audio和video之前，当然是需要先检查下浏览器是否支持。通过下图，可以看出，
Internet Explorer 9+, Firefox, Opera, Chrome, and Safari 都支持video
<figure>
	<img src="{{ site.url }}/images/videos/video-support.png" alt="video-support"></a>
  <center>
	  <figcaption>
      注意：Internet Explorer 8 和之前版本不支持video
    </figcaption>
  </center>
</figure>

检查浏览器是否支持audio或者video也可以使用比较简单脚本，通过动态创建一个video，
然后判断特定函数是否存在：

{% highlight javascript %}
var hasVideo = !!(document.createElement("video").canPlayType());
{% endhighlight %}

上面这段代码通过动态创建一个video元素，然后检查canPlayType()方法是否存在，
接下 "!!" 将判断转为Boolean值，最后就可以通过hasVideo true or
false来判断。

当浏览器不支持video和audio元素时候，可以在代码里面加入备选内容，这样备选提示内容就会在
不支持的浏览器中显示：

{% highlight html %}
<video scr="video.ogg" controls>
您的浏览器不支持HTML5 video
</video>
{% endhighlight %}

如果是要为不支持HTML5 video的浏览器提供其他方式来显示视频，
可以使用同样方式，将代码插入元素里面就可以：

{% highlight html %}
<video scr="video.ogg" controls>
  <object data="video.swf">
    <param name="movie" value="video.swf">
  </object>
</video>
{% endhighlight %}
