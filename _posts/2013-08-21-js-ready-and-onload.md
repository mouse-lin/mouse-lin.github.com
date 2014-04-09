---
layout: post
title: "JS ready and onload"
description: ""
category: JS
tags: [JS]
---
* onload: 当onload事件触发时候，页面上面所有的DOM、样式表、脚本、图片、flash都已经加载完成
* DOMContentLoaded: 当DOMContentLoaded事件触发时候，仅仅当DOM加载完成，不包括样式表、图片、flash

倘若我们需要给一些元素绑定一些事件，但是当页面元素还没有加载完成前，绑定事件函数及已经触发了，那是没有效果的；又例如，我们需要做一种lazy image功能，这个时候我们允许DOM加载完毕，但是图片是没有加载的，这个时候默认放置一张图片或者其他效果

在使用Jquery情况下，你可以使用$(document).ready来代替DOMContentLoaded

在JS情况下，你可以有两种方式来实现$(document).ready：

**使用代码包装:**
	
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
**为你的方法添加DOMContentLoaded事件:**

    document.addEventListener('DOMContentLoaded',function(){/*fun code to run*/})
