---
layout: post
title: "使用pptpd搭建自己的vpn"
modified: 2014-05-19 14:38:58 +0800
tags: [linux vpn]
image:
  feature: abstract-3.jpg
  credit: 
  creditlink: 
comments: true
share: true
---

### 配置安装pptpd提供两种方式

- 自己手动去配置；

- 使用自动脚本（建议，快速搞定，不需要自己去逐个配置）

---

### 自动脚本
{% highlight ruby %}
git clone https://github.com/mouse-lin/pptpd.git
{% endhighlight %}

### 手动配置

* ### 安装pptpd
{% highlight ruby %}
sudu apt-get -y install pptpd
{% endhighlight %}

* ###修改配置脚本

{% highlight ruby %}
cat >/etc/ppp/options.pptpd <<END
name pptpd
refuse-pap
refuse-chap
refuse-mschap
require-mschap-v2
require-mppe-128
ms-dns 8.8.8.8
ms-dns 8.8.4.4
proxyarp
lock
nobsdcomp 
novj
novjccomp
nologfd
END
{% endhighlight %}

* ###修改路由转发
{% highlight ruby %}
cat >> /etc/sysctl.conf <<END
net.ipv4.ip_forward=1
END
sysctl -p
{% endhighlight %}


* ###修改iptables
{% highlight ruby %}
iptables-save > /etc/iptables.down.rules

iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -o eth0 -j MASQUERADE

iptables -I FORWARD -s 192.168.2.0/24 -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1300

iptables-save > /etc/iptables.up.rules

cat >>/etc/ppp/pptpd-options<<EOF
pre-up iptables-restore < /etc/iptables.up.rules
post-down iptables-restore < /etc/iptables.down.rules
EOF
{% endhighlight %}

* ###配置登录vpn用户账号
{% highlight ruby %}
cat >/etc/ppp/chap-secrets <<END
test pptpd test *
END
{% endhighlight %}
