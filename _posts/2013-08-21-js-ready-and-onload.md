---
layout: post
title: "JS ready and onload"
description: ""
category: JS
tags: [JS]
---
#### onload与DOMContentLoaded区别

* onload: 当onload事件触发时候，页面上面所有的DOM、样式表、脚本、图片、flash都已经加载完成,
  并且多个onload只会有一个被触发
* DOMContentLoaded: 当DOMContentLoaded事件触发时候，仅仅当DOM加载完成，不包括样式表、图片、flash

#### 实际使用到的场景

1. 假如我们需要给元素绑定事件，但当页面元素还没有加载完成前，绑定事件函数及已经触发了，那是没有效果的；
2. 或者，lazy image功能，当我们允许DOM加载完毕，但图片并没有完成加载的，这时需要默认放置一张图片或者其他效果

#### 代码例子

  * 在使用Jquery情况下，你可以使用$(document).ready来代替DOMContentLoaded

  * 在JS情况下，你可以有两种方式来实现$(document).ready：

**ready实现的代码:**

{% highlight javascript %}
	function bindReady(){
  		if ( readyBound ) return;
  		readyBound = true;

  		// Mozilla, Opera and webkit nightlies currently support this event
  		if ( document.addEventListener ) {
  			// Use the handy event callback
  			document.addEventListener( "DOMContentLoaded", function(){
  			document.removeEventListener( "DOMContentLoaded", arguments.callee, false );
  				jQuery.ready();
  			}, false );

  		// If IE event model is used
  		} else if ( document.attachEvent ) {
  			// ensure firing before onload,
  			// maybe late but safe also for iframes
  			document.attachEvent("onreadystatechange", function(){
  				if ( document.readyState === "complete" ) {
  				document.detachEvent( "onreadystatechange", arguments.callee );
  				jQuery.ready();
  			}
  		});
  			// If IE and not an iframe
  			// continually check to see if the document is ready
  		if ( document.documentElement.doScroll && window == window.top ) (function(){
  			if ( jQuery.isReady ) return;

  			try {
  				// If IE is used, use the trick by Diego Perini
  				// http://javascript.nwbox.com/IEContentLoaded/
  				document.documentElement.doScroll("left");
  			} catch( error ) {
  				setTimeout( arguments.callee, 0 );
  				return;
  			}

  			// and execute any waiting functions
  			jQuery.ready();
  			})();
  	  	}

  		// A fallback to window.onload, that will always work
  		jQuery.event.add( window, "load", jQuery.ready );
	}
{% endhighlight %}

**为你的方法添加DOMContentLoaded事件:**

{% highlight javascript %}
document.addEventListener('DOMContentLoaded',function(){/*fun code to run*/})
{% endhighlight %}
