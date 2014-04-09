---
layout: post
title: "Munin Alert Emial"
description: ""
category: Server
tags: [Server, SA]
---

### Munin alert

Munin alert支持几种方式

***Email alert***: 

但是这种在我们天朝下面是基本不能用的，所以需要自己去写脚本发送email

***Syslog Alert***: 

To send syslog message with priority use a command such as this:
	
    contact.syslog.command logger -p user.crit -t "Munin-Alert"
		
***Alerts to or through external scripts***:
	 
To run a script (in this example, 'script') from Munin use a command such as this in your munin.conf. Make sure that:
	
  1. There is NO space between the '>' and the first 'script'
  	
  2. 'script' is listed twice and
  	
  3. The munin user can find the script -- by either using an absolute path or putting the script somewhere on the PATH -- and has permission to execute the script.
	

***Code***:
  
    contact.person.command >script script

This syntax also will work (this time, it doesn't matter if there is a space between '|' and the first 'script' ... otherwise, all the above recommendations apply):

    contact.person.command | script script


***我们上面所说的自己写脚本发送email就是通过这种方式详细可以参考我写的一个脚本***：
	
[Munin-Alert-Email](https://github.com/mouse-lin/munin-alert-email)

### Test & Debug

根据上面Munin-Alert-Email README 使用脚本，然后接下来就是测试和Debug，Munin在跑munin-plugins时候，用的是 root用户，而在执行alert时候，是munin用户，这样，你就需要保证你执行脚本在munin下是可以执行的

* /var/log/munin/munin-limits.log: 查看每次limit log
* /var/lib/munin/limits：每次limits输出
* /usr/share/munin/munin-limits --debug： debug limits，如果邮件无法发送，你可以尝试下这个debug，如果出现lock，这个时候你就需要把对应在跑的 munin-limits 给 kill 掉

最后，就等待5分钟或者Debug limit之后，就可以接受到邮件

[Munin](http://munin-monitoring.org/wiki/HowToContact)
