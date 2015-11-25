Title: Digitalocean搭建wordpress不完全攻略
Date: 2015-09-05 
Category: Geek
Tags: VPS, WordPress, digitalocean
Slug: digitalocean-wordpress-experience
Authors: hateonion
Summary: WordPress是一款基于PHP的开源的轻量级博客框架，由于内部高度模块化，非常适合像我这种PHP不怎么懂的人进行个人博客的搭建。digitalocean是一款性价比很高的VPS服务器，最低的套餐每个月仅5$。VPS比起虚拟主机来自主性强大了不少，很适合初学者学习Linux和运维方面的一系列知识。

#### 关于digitalocean的购买和一些基本知识我这里就不再作介绍了，下面来看如何在VPS上搭建属于自己的WordPress。

## 一、一切网站都需要一个环境——lnmp（或lamp等）。

LNMP的环境配置起来在使用了_**[LNMP一键安装包](http://lnmp.org/) **_之后就很简单了

具体使用方法如下

1、使用putty（windows下）或者ssh命令登陆到你的VPS

2、运行以下命令 `wget -c http://soft.vpser.net/lnmp/lnmp1.2-full.tar.gz && tar zxf lnmp1.2-full.tar.gz && cd lnmp1.2-full && ./install.sh lnmp`

没有wget的话可以 yum 或者apt-get 安装一下

3、接下来会依次需要输mysql root密码，mysql版本,php版本等选项，没有特殊需求的话一律回车就可以。当出现如下图像时表示幻镜搭建成功

[![lnmp-1.2-install-sucess2](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-install-sucess2-300x268.png)](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-install-sucess2.png)

## 二、无主机，无网站

当环境搭建完之后，我们需要搭建虚拟主机来为WordPress做好准备。

1、ssh到主机 运行`lnmp vhost add`

![lnmp-1.2-vhost-add-1](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-vhost-add-1-300x89.png)

再这里输入你的域名

[![lnmp-1.2-vhost-add-2](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-vhost-add-2-300x66.png)](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-vhost-add-2.png)

如果你还要添加更多域名，在这里在输入（我输入了www.域名.com，因为www与顶级域名解析是不同的）

2、设定网站目录，这里直接进行回车采用默认目录就好。

3、设定伪静态，自己在搭建的时候并没有怎么注意这个，只是看到有个WordPress选项就填写了，伪静态对SEO和网站提速都有帮助，建议输入wordpress然后回车

[![lnmp-1.2-vhost-add-5](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-vhost-add-5-300x80.png)](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-vhost-add-5.png)

4、设定数据库

在一个回车默认日志启用后 需要设定数据库

[![lnmp-1.2-vhost-add-7](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-vhost-add-7-300x67.png)](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-vhost-add-7.png)

当出现以下界面是要注意需要仔细设定，因为这是后面影响到WordPress一键安装的一些前提条件。

5、

[![lnmp-1.2-vhost-add-9](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-vhost-add-9-300x162.png)](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/lnmp-1.2-vhost-add-9.png)

出现如下图片，表示设定完成，注意记录好里面所包含的信息，后面会用到。

### 三、来吧，WordPress

在一系列准备工作做好后，我们终于可以着手WordPress的搭建了。

1、首先还是ssh到vps  运行 `wget https://cn.wordpress.org/wordpress-4.3-zh_CN.zip`

下载安装包  如需英文 可以  `wget https://cn.wordpress.org/wordpress-4.3-zh_CN.zip`

2、运行 `unzip wordpress-4.3-zh_CN.zip`进行解包

3、运行   `cp -rf wordpress/* /home/wwwroot/你的域名`

将WordPress复制到虚拟主机的目录下

4、进入浏览器 输入 www.你的域名.com   如果没有问题的话 会出现如下界面

[![setup-config](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/setup-config-300x208.png)](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/setup-config.png)

那么恭喜你 你离成功就只差一步了

输入第第二步中所记录下来的mysql名称 用户名 密码 数据库主机和表前缀可以按默认 应用之后

呈现在你眼前的

[![2013-10-28_2355](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/2013-10-28_2355-300x180.png)](http://7xn8xc.com1.z0.glb.clouddn.com/wp-content/uploads/2015/09/2013-10-28_2355.png)

就是你自己的wordpress网站啦～

_引用：[http://lnmp.org/](http://lnmp.org/) ————lnmp安装和制作_

_[https://cn.wordpress.org/](https://cn.wordpress.org/)—— WordPress官方安装教程_
