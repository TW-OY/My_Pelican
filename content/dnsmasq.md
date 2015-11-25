Title: 使用dnsmasq,智能提速+防dns污染
Date: 2015-10-01 
Category: Geek
Tags: VPS, DNS, 上网
Slug: dnsmasq-accelerate-anti-dns-pollution
Authors: hateonion
Summary: 最近一直在忙找工作，关于路由器的折腾也就放在了一边。昨天趁着闲着准备看个Youtube，但是却发现浏览器怎么也打不开Youtube的网页。准备google，发现google系网页都解析不了。一ping才发现，ip变成了101::1234,妈了个鸡，这是活生生的dns污染！！！身为一个爱好自由和平的人这怎么能忍（哼）。让我用dnsmasq来治治你～" 


## 什么是dnsmasq?

Dnsmasq 提供 DNS 缓存和 DHCP 服务功能。作为域名解析服务器(DNS)，dnsmasq可以通过缓存 DNS 请求来提高对访问过的网址的连接速度。作为DHCP 服务器，dnsmasq 可以用于为局域网电脑分配内网ip地址和提供路由。

**在这里我们着重使用的是其dns缓存功能，dhcp功能就不深究了**

## dnsmasq的工作原理是什么？

字面上的意思可能不够很好理解，这里我自己画了个简单的示意图。![2015-10-01 16:13:24屏幕截图](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/10/2015-10-01-161324屏幕截图.png-w650)

大概的意思就是这样，通过dnsmasq从本地就进行一次dns解析，从而使域名避免被污染。

## 如何配置dnsmasq?

安装我就不细说了 各种yum apt-get opkg等都能很方便的进行安装。 安装完后终端键入 `vi /etc/dnsmasq.conf`进行编辑。里面会有很多注释，想认真学习的话推荐认真阅读，如果只是想要简单的配置的话，清楚掉全部内容后复制以下内容


	# /etc/dnsmasq.conf
	# Set up your local domain here
	domain=raspberry.local
	resolv-file=/etc/resolv.dnsmasq
	min-port=4096
	server=/taobao.com/8.8.8.8
	address=/google.com/216.58.192.46
	# Max cache size dnsmasq can give us, and we want all of it!
	cache-size=10000
	# Below are settings for dhcp. Comment them out if you dont want
	# dnsmasq to serve up dhcpd requests.
	# dhcp-range=192.168.0.100,192.168.0.149,255.255.255.0,1440m
	# dhcp-option=3,192.168.0.1
	# dhcp-authoritative`


其中 **resolv-file** 是保存你上级dns服务的文件的地址。上级dns服务器是指你本地的dnsmasq未设置该域名的解析时，解析会自动发向上级dns服务器进行解析。设置方法如下： `nameserver 上级dns的地址` 而另外两个我们需要关心的是**server=/taobao.com/8.8.8.8**和**address=/google.com/216.58.192.46** `server=/域名/dns服务器地址` 这条命令是指定特定的域名采用特定的dns服务器解析 `address=/域名/域名强制解析到的ip地址` 这条命令是强制域名解析到指定ip地址，这条命令用好了可以防止dns污染，用的不好也能进行dns劫持。

* * *

**至此，防止dns污染就告一段落了。但是细心的人可能已经注意到了，server=这条命令是可以进行指定dns服务器解析的，那么我们是否可以进行特定域名选择特定的dns服务器来解析达到加速解析的目的呢？答案是肯定的！**

## 如何用dnsmasq进行dns智能解析？

上面也说了 原理就是利用`server=/域名/dns服务器地址`这条命令来进行。这里推荐一个github上的项目 [https://github.com/felixonmars/dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list) 也就是著名的ChinaDns，里面的列表会定期更新，大家直接复制后粘贴即可。

## 最后呢，分享一下我的具体构建方案

由于我的整个网络环境是在ipv6下通过socks5代理来上网，所以一般的v4服务器本地ping是ping不通的，所以就不能只设置路由器端一方的dnsmasq，要使用二级dnsmasq(可能有朋友有更好的方法只需要一级，欢迎私信我来交流)。系统结构如下

![2015-10-01 16:46:54屏幕截图](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/10/2015-10-01-164654屏幕截图.png-w650)

## 至此，一切就大功告成了，接下来，就愉快的上网吧～