---
layout: post
title: "oauth2 provider"
description: ""
category: 
tags: [Ruby]
image:
  feature: abstract-10.jpg
comments: true
share: true  
---

### 介绍

OAuth（开放授权）是一个开放标准，允许用户让第三方应用访问该用户在某一网站上存储的私密的资源（如照片，视频，联系人列表），而无需将用户名和密码提供给第三方应用

### 认证过程

- 使用OAuth进行认证和授权的过程如下所示:
- 用户访问客户端的网站，想操作自己存放在服务提供方的资源。
- 客户端向服务提供方请求一个临时令牌。
- 服务提供方验证客户端的身份后，授予一个临时令牌。
- 客户端获得临时令牌后，将用户引导至服务提供方的授权页面请求用户授权。在这个过程中将临时令牌和客户端- 的回调连接发送给服务提供方。
- 用户在服务提供方的网页上输入用户名和密码，然后授权该客户端访问所请求的资源。
- 授权成功后，服务提供方引导用户返回客户端的网页。
- 客户端根据临时令牌从服务提供方那里获取访问令牌 。
- 服务提供方根据临时令牌和用户的授权情况授予客户端访问令牌。
- 客户端使用获取的访问令牌访问存放在服务提供方上的受保护的资源

![oauth2](/assets/flow.png "Title")

![oauth2](/assets/server_flow.png "Title")
 

### Flow

**六种流程**:

- User-Agent Flow 客户端运行于用户代理内（典型如web浏览器）。
- Web Server Flow 客户端是web服务器程序的一部分，通过http request接入，这是OAuth 1.0提供的流程的简化版本。
- Device Flow 适用于客户端在受限设备上执行操作，但是终端\a户单独接入另一台电脑或者设备的浏览器
- Username and Password Flow 这个流程的应用场景是，用户信任客户端处理身份凭据，但是仍然不希望客户端储存他们的用户名和密码，这个流程仅在用户高度信任客户端时才适用。
- Client Credentials Flow 客户端适用它的身份凭据去获取access token，这个流程支持2-legged OAuth的场景。
- Assertion Flow 客户端用assertion去换取access token，比如SAML assertion。

**其他网站例子**: 

- 豆瓣 User-Agent、Web Server、Native Application Username and Password Flow[Wiki](http://developers.douban.com/wiki/?title=oauth2)
- 淘宝 User-Agent、Web Server、Native Application、登陆账号退出流 Username and Password Flow[Wiki](http://open.taobao.com/doc/detail.htm?id=118)
- Sina User-Agent(站内应用)、Web Server、Native Application Username and Password Flow[Wiki](http://open.weibo.com/wiki)

**选择**:

- Username and Password Flow(适合官方第一应用)
- Web Server Flow(适合第三接入，比较常用)

### Draft

- [http://tools.ietf.org/html/draft-ietf-oauth-v2-25](http://tools.ietf.org/html/draft-ietf-oauth-v2-25)
- [http://oauth.net/2/](http://oauth.net/2/)


### Ruby Provider 实现

**Client Credential Flow**:
  
- 过程
	
![oauth2](/assets/ownresource.png "Title")
	
- 请求参数：
   
  client_id: 必选参数，应用的唯一标识
   
  client_secret: 必选参数，应用的唯一标识
   
  user_name: 用户账号
   	
  user_password: 用户密码
   	
  grant_type: password
  

- 例子：

{% highlight ruby %}
http://server/oauth/token?client_id&client_secret&user_name&user_password 

method: post
{% endhighlight %}
 

- 返回参数：
  
{% highlight ruby %}
{ 		
"access_token":"de6780bc506a0446309bd9362820ba8aed28aa506c71eedbe1c5c4f9dd350e54",
"token_type": "bearer", 
"expires_in": 7200,
"refresh_token":"8257e65c97202ed1726cf9571600918f3bffb2544b26e00a61df9897668c33a1"
}
{% endhighlight %}

access_token不再长期有效。在授权获取access_token时会一并返回其有效期，也就是返回值中的expires_in参数。
	
在access_token使用过程中，如果服务器返回错误：“access_token_has_expired ”，此时，说明access_token已经过期，就需要发送refresh_token的方式来换取新的access_token和refresh_token。

- 请求参数
 
  client_id: 必选参数，应用的唯一标识
   
  client_secret: 必选参数，应用的唯一标识
      	
  grant_type: refresh_token
  
  refresh_token: refresh_token
 
- 例子

{% highlight ruby %}
    https://server/auth2/token?client_id=0b5405e19c58e4cc21fc11a4d50aae64&client_secret=edfc4e395ef93375&redirect_uri=https://www.example.com/back&grant_type=refresh_token&refresh_token=5d633d136b6d56a41829b73a424803ec
    
    method: post
{% endhighlight %}

	
- 返回参数

{% highlight ruby %}
    {
    "access_token":"0e63c03dfb66c4172b2b40b9f2344c45",
    "expires_in":3920,
    "refresh_token":"84406d40cc58e0ae8cc147c2650aa20a",
    }
{% endhighlight %}

**Rails gem**:

- https://github.com/applicake/doorkeeper
- https://github.com/nov/rack-oauth2
- https://github.com/opro/opro
- https://github.com/songkick/oauth2-provider
- https://github.com/freerange/oauth2-provider
- https://github.com/socialcast/devise_oauth2_providable
- https://github.com/intridea/oauth2
