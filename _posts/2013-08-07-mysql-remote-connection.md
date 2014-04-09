---
layout: post
title: "mysql-remote-connection"
description: ""
category: MySQL
tags: [SQL]
---

## MySQL 开启与关闭远程访问

### (1)通过MySQL用户去限制访问

**权限系统目的**:

MySQL基于安全考虑root账户一般只能本地访问，但是在开发过程中可能需要打开root的远程访问权限，今天介绍的就是如何开启和关闭Mysql远程访问

MySQL权限系统的主要功能是证实连接到一台给定主机的用户，并且赋予该用户在数据库上的SELECT、INSERT、UPDATE和DELETE权限。
附加的功能包括有匿名的用户并对于MySQL特定的功能例如LOAD DATA INFILE进行授权及管理操作的能力。

**权限系统原理**:

MySQL权限系统保证所有的用户只执行允许做的事情。当你连接MySQL服务器时，你的身份由你从那儿连接的主机和你指定的用户名来决定。连接后发出请求后，系统根据你的身份和你想做什么来授予权限。

MySQL在认定身份中考虑你的主机名和用户名字，是因为几乎没有原因假定一个给定的用户在因特网上属于同一个人。例如，从office.com连接的用户joe不一定和从elsewhere.com连接的joe是同一个人。MySQL通过允许你区分在不同的主机上碰巧有同样名字的用户来处理它：你可以对joe从office.com进行的连接授与一个权限集，而为joe从elsewhere.com的连接授予一个不同的权限集。

阶段1：服务器检查是否允许你连接。
阶段2：假定你能连接，服务器检查你发出的每个请求。看你是否有足够的权限实施它。例如，如果你从数据库表中选择(select)行或从数据库删除表，服务器确定你对表有SELECT权限或对数据库有DROP权限。
如果连接时你的权限被更改了(通过你和其它人)，这些更改不一定立即对你发出的下一个语句生效。MySQL权限是保存在cache中，这个时候就你需要执行

	flush privileges;

**开启远程访问**:

- 更新用户
		
	use mysql;
	
	update user set host = "%" where user = "root";
	
	flush privileges;

- 添加用户

	use mysql;
	
	insert into user(host, user, password) values("%", "root", password("yourpassword"))
	
	grant all privileges on *.* to 'root'@'%' with grant option #赋予任何主机访问数据库权限
	
	flush privileges;
		
	
**关闭远程访问**:
	
	use mysql;
	
	update user set host = "localhost" where user = "root" and host= "%";
	
	flush privileges;
	
**查看用户权限**:

	use information_schema;
	
	select * from user_privileges;

**查看当前mysql用户**:

	use mysql;
	
	select user, host from user;

**更新用户**:

	update mysql.user set password=password('新密码') where User="phplamp" and Host="localhost";
	
	flush privileges;
	
**删除用户**:

	DELETE FROM user WHERE User="phplamp" and Host="localhost";
	
	flush privileges;
		
**user host指定方法**:

- Host值可以是主机名或IP号，或'localhost'指出本地主机。
- 你可以在Host列值使用通配符字符“%”和“_”。
- host值'%'匹配任何主机名，空Host值等价于'%'。它们的含义与LIKE操作符的模式匹配操作相同。例如，'%'的Host值与所有主机名匹配，而'%.mysql.com'匹配mysql.com域的所有主机。

**ip地址例子**:

    192.0.0.0/255.0.0.0(192 A类网络的任何地址)
    192.168.0.0/255.255.0.0(192.168 A类网络的任何地址)
    192.168.1.0/255.255.255.0(192.168.1 C类网络的任何地址)
    192.168.1.1(只有该IP)
    		
**mysql doc**:

[http://dev.mysql.com/doc/refman/5.1/zh/database-administration.html](http://dev.mysql.com/doc/refman/5.1/zh/database-administration.html)

### (2)通过IpTable

iptables是一款防火墙软件。它在Ubuntu系统中是默认安装的。通常情况下，iptables随系统一起被安装，但没有对通信作任何限制，因此防火墙并没有真正建立起来。

**iptables帮助**:

	sudo iptables -h #下面全部使用root用户调用指令
		
**查看iptables**:

	iptables -L
	
	Chain INPUT (policy ACCEPT)
	target     prot opt source               destination
	
	Chain FORWARD (policy ACCEPT)
	target     prot opt source               destination
	
	Chain OUTPUT (policy ACCEPT)
	target     prot opt source               destination

可以看到，上面规则都是空的

**允许已建立的连接接收数据**:

	iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
		
**开放指定的端口**:

接下来，我们可以尝试开放 ssh 22端口，告诉iptables允许接受到所有目标端口为22的tcp报文通过

	iptables -A INPUT -p tcp -i eth0 --dport ssh -j ACCEPT
		
执行上面的命令，一条规则会被追加到INPUT规则表的末尾（-A表示追加）。根据这条规则，对所有从接口eth0（-i指出对通过哪个接口的报文运用此规则）接收到的目标端口为22的报文，iptables要执行ACCEPT行动（-j指明当报文与规则相匹配时应采取的行动）。

**开放80端口**:

	iptables -A INPUT -p tcp -i eth0 --dport 80 -j ACCEPT
		
		
这时，你再次去看看规则表中内容，你会发现有

	iptables -L

	Chain INPUT (policy ACCEPT)
	target     prot opt source               destination
	ACCEPT     all  --  anywhere             anywhere            state RELATED,ESTABLISHED
	ACCEPT     tcp  --  anywhere             anywhere            tcp dpt:ssh
	ACCEPT     tcp  --  anywhere             anywhere            tcp dpt:www
		
通过上述命令，我们已经代开了SSH和web服务的相应的端口，但由于没有阻断任何通信，因此所有的报文都能通过，所以接下来，我们就可以尝试阻断3306 mysql端口

**阻断通讯**:

	iptables -A INPUT  -p tcp -i eth0 --dport 3306 -j DROP
	
通过这样指令，就可以阻断任何访问3306报文，但是这样就有一个问题，就是可能连我们自己信任的主机都无法访问mysql了，所以，我们需要编辑下iptables，添加特定允许访问的主机

	iptables -I INPUT 4 -p tcp -s ip_address -i eth0 --dport 3306 -j ACCEPT
	
好了，通过上面，我们把该指令插入到规则表里第四行，然后允许特定ip访问3306端口

**Logging记录**:

如果希望被丢失的报文记录到syslog中，最简单的方法可以这样做:

	iptables -I INPUT 5 -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " --log-level 7

**保存设置**:

机器重启后，iptables中的配置信息会被清空。您可以将这些配置保存下来，让iptables在启动时自动加载，省得每次都得重新输入。iptables-save和iptables-restore 是用来保存和恢复设置的。

先将防火墙规则保存到/etc/iptables.up.rules文件中

	iptables-save > /etc/iptables.up.rules

然后修改脚本/etc/network/interfaces，使系统能自动应用这些规则（最后一行是我们手工添加的）。

	auto eth0
	iface eth0 inet dhcp
	pre-up iptables-restore < /etc/iptables.up.rules

当网络接口关闭后，您可以让iptables使用一套不同的规则集。

	auto eth0
	iface eth0 inet dhcp
	pre-up iptables-restore < /etc/iptables.up.rules
	post-down iptables-restore < /etc/iptables.down.rules
	
**技巧(Tips)**:

大多数人并不需要经常改变他们的防火墙规则，因此只要根据前面的介绍，建立起防火墙规则就可以了。但是如果您要经常修改防火墙规则，以使其更加完善，那么您可能希望系统在每次重启前将防火墙的设置保存下来。为此您可以在/etc/network/interfaces文件中添加一行：

	pre-up iptables-restore < /etc/iptables.up.rules
	post-down iptables-save > /etc/iptables.up.rules
