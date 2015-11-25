Title: 在WNDR4300上搭建基于shadowsocks校园网免流量上网
Date: 2015-09-07
Category: Geek
Tags: IPV6, openwrt, ss
Slug: openwrt-shadowsocks-free-ipv6
Authors: hateonion
Summary: 学校的网是流量计费，10块钱6G超限后死贵，坑爹的要死。还好有神奇的shadowsocks.本来在极路由上可以很方便的进行IPV6+shadowsocks插件免流量上网。最近换了个网件的WNDR4300,想着openwrt配置起来肯定很简单，但实际上却折腾了四天才勉强能用。




## 一、服务器端的shadowsocks 服务端搭建

这个还是很简单的，因为现在有了众多大神做好的一键安装包可以wget或者git。

我在这里采用的是 **秋水逸冰 **大大的 [shadowsocks-python](https://teddysun.com/342.html)一键安装包

配置起来还是很简单的 按照秋水逸冰的安装教程

### 1、ssh到你的VPS   在终端键入以下命令

<div class="highlight">

<pre>wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log
</pre>

</div>

可以一路回车 在后面再进行配置文件的修改

当出现以下文字时 表示shadowsocks服务端已经配置成功了。

<div class="highlight">

<pre>Congratulations, shadowsocks install completed!
Your Server IP:your_server_ip
Your Server Port:your_server_port
Your Password:your_password
Your Local IP:127.0.0.1
Your Local Port:1080
Your Encryption Method:aes-256-cfb
Welcome to visit:http://teddysun.com/342.html Enjoy it!
</pre>

</div>

### 2、进行配置文件的修改

在终端键入 `vi /etc/shadowsocks.json`

直接给出多用户配置如下

<div class="highlight">

<pre>{
    "server":"::",  "这里这么设置是保证同时监听V6和V4端口"
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
         "8989":"password0",
         "9001":"password1",
         "9002":"password2",
         "9003":"password3",
         "9004":"password4"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
</pre>

</div>

下面是shadowsocks的一些基本命令

<div class="highlight">

<pre>"启动"：/etc/init.d/shadowsocks start
"停止"：/etc/init.d/shadowsocks stop
"重启"：/etc/init.d/shadowsocks restart
"查看状态"：/etc/init.d/shadowsocks status
</pre>

</div>

运行启动命令 你的shadowsocks就运行在服务器端啦～

## 二、openwrt shadowsocks+IPV6环境搭建

首先是IPV6环境的搭建，现在很多集成好的固件都去掉了IPV6功能，首先选择固件上以支持IPV6的集成固件为佳。

本人选择的是恩山wifi论坛 [明月大大的春节版固件](http://www.right.com.cn/forum/thread-139399-1-1.html)

### 1、首先需要确认如果支持ipv6接入

如果确认支持无误，ssh到路由器端，运行 `ping6 [2a00:1450:8002::93]`

应该是能够Ping通的 如果ping不通的话，很多固件里集成了多种ipv6协议的

![2015-09-07 23:39:22屏幕截图](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/2015-09-07-233922屏幕截图-300x133.png) 选择你学校对应的协议，具体设置的话请自行移步百度谷歌。

### 2、设置shadowsocks客户端

在能ping通的情况下  进行shadowsocks的设置   设置如下图

![2015-09-07 23:47:11屏幕截图](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/2015-09-07-234711屏幕截图-300x191.png)

##### 注意：1.服务器我没有填写VPS的IPV6地址 因为这个版本的shadowsocks好像有bug 无法解析V6服务器地址 所以我在DNSPOD做了一个域名解析

##### 2\. 采用全局代理是为了免费上网，智能FQ的话选择列表并配合诸如Chinadns之类的软件进行设置即可

##### 3.打开UDP转发是为了更好的支持移动端，访问国内据说也能加速，DNS地址选择的是DNSPOD的公共DNS服务器，也可自行替换

### 3、进行全局DNS服务器的设置

我就是在这里卡了很久，因为shadowsocks连上后一直DNS解析错误。按照官方WIKI（[http://wiki.openwrt.org/start?do=search&id=IPV6+DNS](http://wiki.openwrt.org/start?do=search&id=IPV6+DNS)）进行设置，可以使用。但是发现每次重启后 /etc/resolv.conf 和dhcp里面的dnsmasq都会进行重置，需要再次配置很麻烦。

进行鼓捣之后解决方法如下

在192.168.1.1管理页面依次 接口——wan6——高级设置 设置wan6口的DNS服务器  我选择的是谷歌的DNS服务器 也可选择其他 支持双栈接入就行   在这个页面有一些DNS服务器可供选择      [http://www.douban.com/note/188058161/](http://www.douban.com/note/188058161/)

![2015-09-07 23:55:24屏幕截图](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/2015-09-07-235524屏幕截图-300x35.png)

至此，我们就可以自由的免费上（F)网（Q）了～

### 4、来点优化

我们当然不应该满足于此啦，配置在VPS端的shadowsocks可以进行优化来达到提速，我选择的方案是V2EX上大家比较认可的锐速

下面是官网

[http://www.serverspeeder.com/](http://www.serverspeeder.com/)

具体介绍原理我就不说了，下面简要说下安装过程：

依次输入以下代码

<div class="highlight">

<pre>wget http://my.serverspeeder.com/d/ls/serverSpeederInstaller.tar.gz
tar xzvf serverSpeederInstaller.tar.gz
bash serverSpeederInstaller.sh
</pre>

</div>

会提示输入账号密码输入就是了 其余一路回车 需要注意的一个地方是自动启动 这里选上自动启动 其他没什么要注意的地方了

接下来如果你的VPS内核正确的话就会自动安装配置了

然后   享受加速之后的网络吧～

### 让我们一起 FUCK THE GFW   FUCK THE 校园网！