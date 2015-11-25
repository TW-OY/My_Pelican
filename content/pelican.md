Title: 教你将博客从WordPress迁移到Pelican
Date: 2015-10-06
Category: Geek
Tags: WordPress, pelican, github
Slug: Wordpress-to-Pelican-github-Pages
Authors: hateonion
Summary: 自己买的DO的服务器是512M内存的，跑了ss服务+dnsmasq+锐速后再跑LNMP就有点吃力了。恰巧在浏览博客的时候发现了网站原来还分静态和动态之分。静态所占用的资源比动态要少很多，并且静态博客可以很方便的托管在github.Pages上。常用的静态框架有很多，比如jekyll、Hexo等等，在这里由于自己最近在学习python,所以选用了一款python的开源博客框架Pelican.




<h3>安装Pelican</h3>
<p>如果你的电脑安装了pip工具的话 安装Pelican键入如下代码即可</p>
<p><code>pip install pelican markdown</code></p>
<p>而更多时候，这种安装方法似乎不太好使，会有各种关联包的问题，因此我更推荐使用apt-get，yum等方式来进行安装</p>
<div class="highlight"><pre>sudo apt-get install pelican
sudo apt-get install markdown
</pre></div>


<p>当Pelican安装好后，就可以开始进行Pelican的本地配置了</p>
<h4>Q：为什么要配置本地Pelican</h4>
<p>Pelican本地环境配置好后，你才能生成静态的html文件。静态博客其实就是一堆的html、js和css堆起来的，你要做的只是在本地把这些东西生成好，然后发布上去就行了。此外在发布之前，你可以很方便的在本地进行预览，而不是当你发布上去之后发现哪个地方没弄好又要重新pull下来进行修改后再push上去</p>
<p>首先键入以下命令 创建你的Pelican</p>
<div class="highlight"><pre>mkdir your_site_name
cd your_site_name
pelican-quickstart
</pre></div>


<p>配置过程一直回车就可以了，后面如果有自己不满意的地方可以在pelican.conf文件中进行修改。</p>
<h3>尝试发布你的第一篇博文</h3>
<p>pelican的markdown标注有很严格的要求，要求你在每篇文章的开始以如下格式创建说明。</p>
<div class="highlight"><pre><span class="n">Title</span><span class="o">:</span> <span class="n">Title</span>                    <span class="s2">&quot;标题&quot;</span>
<span class="n">Date</span><span class="o">:</span> <span class="mi">2010</span><span class="o">-</span><span class="mi">12</span><span class="o">-</span><span class="mi">03</span> <span class="mi">10</span><span class="o">:</span><span class="mi">20</span>       <span class="s2">&quot;时间 格式一定要这样&quot;</span>
<span class="n">Category</span><span class="o">:</span> <span class="n">Python</span>                <span class="s2">&quot;分类&quot;</span>
<span class="n">Tags</span><span class="o">:</span> <span class="n">pelican</span><span class="o">,</span> <span class="n">publishing</span>       <span class="s2">&quot;标签&quot;</span>
<span class="n">Slug</span><span class="o">:</span> <span class="n">my</span><span class="o">-</span><span class="kd">super</span><span class="o">-</span><span class="n">post</span>         <span class="s2">&quot;固定链接（你的html对应的名字）&quot;</span>
<span class="n">Author</span><span class="o">:</span> <span class="n">Alexis</span> <span class="n">Metaireau</span>        <span class="s2">&quot;作者&quot;</span>
<span class="n">Summary</span><span class="o">:</span> <span class="n">Short</span> <span class="n">version</span> <span class="k">for</span> <span class="n">index</span> <span class="n">and</span> <span class="n">feeds</span>  <span class="s2">&quot;描述，作为主页的缩略显示&quot;</span>

<span class="n">This</span> <span class="k">is</span> <span class="n">the</span> <span class="n">content</span> <span class="n">of</span> <span class="n">my</span> <span class="kd">super</span> <span class="n">blog</span> <span class="n">post</span><span class="o">.</span>      <span class="s2">&quot;正文&quot;</span>
</pre></div>


<p>该文章创建好后，将其保存在content目录中（以后的文章都保存在此）进入你的Pelican主目录，键入</p>
<div class="highlight"><pre>pelican content
./develop_server.sh start
</pre></div>


<p>这样你在浏览器访问<strong>localhost:8000</strong>就应该能看到你自己的网站了～
<img alt="default_my_site" src="http://7xn8xc.com1.z0.glb.clouddn.com/images_blog2015-10-06%2016%3A21%3A59屏幕截图.png-w650" /></p>
<h2>将你的博客发布到github.Pages</h2>
<h3>创建一个github.Pages</h3>
<h4>Q:什么是github.Pages</h4>
<p>github.Pages是github提供给开发者的类似于网页托管的服务。每个注册用户可以生成一个属于自己的用户Pages。其形式和一般的repository一样，不同的是，你将你的静态博客的内容推送上去的话，这些内容可以被智能解析，你通过访问<strong>yourname.github.io(com)</strong>就能够访问你的博客了。</p>
<p>github的账号注册我就不多提了，进入github 新建repository
<img alt="build_repository" src="http://7xn8xc.com1.z0.glb.clouddn.com/images_blog2015-10-06%2016%3A38%3A59屏幕截图.png" /></p>
<p>进入setting</p>
<p><img alt="setting" src="http://7xn8xc.com1.z0.glb.clouddn.com/images_blog2015-10-06%2016%3A41%3A44屏幕截图.png" /></p>
<p>进入options选项 找到github Pages 选择 <strong>launch automatic page generator</strong>
<img alt="generator" src="http://7xn8xc.com1.z0.glb.clouddn.com/images_blog2015-10-06%2016%3A43%3A18屏幕截图.png" /></p>
<p>选择一个合适的主题，起好名字，就ok啦～这时候访问<strong>username.github.io</strong>就能看到属于你的网站了</p>
<h3>发布你的博客</h3>
<p>键入如下命令</p>
<div class="highlight"><pre>cd yoursitename/output
git init
git remote add origin https://github.com/username/username.github.com.git
git pull
git add .
git commit -m &quot;说明&quot;
git push origin master
</pre></div>


<p>如果你发现各种本地主机文件不同步的话，我推荐你先按照以下操作，把master分支下的所有文件删除，在进行本地博客的推送（output里面的文件要先进行备份噢）</p>
<div class="highlight"><pre>cd yoursitename/output
git init
git remote add origin https://github.com/username/username.github.com.git
git pull
“删除掉全部文件”
git commit -m &quot;说明&quot;
git push
</pre></div>


<p>等待删除完后，再把你的所有的output文件放进这个文件夹然后</p>
<div class="highlight"><pre>git add .
git commit -m &quot;说明&quot;
git push
</pre></div>


<p>现在再看看访问你的<strong>username.github.io(com)</strong>是不是已经和你的Pelican一样了～</p>
<h3>设置域名解析</h3>
<p>关于域名解析的设置不是本文的重点之列，可以自行百度，我们所要注意的是如果需要域名解析到你自己的域名，需要在master分支下添加名为<strong>CNAME</strong>的文件，文件内容为<code>yourdomain</code>，这样的话，你通过在dnspod或者其他域名解析服务商处设置A记录（A记录的服务器地址需要dig或者ping得出）就能正确解析了～</p>
<h2>从WordPress搬家到Pelican</h2>
<p>这里我借鉴了一篇很好的文章，他是迁移到jekyll，而我是Pelican，其实都是大同小异。</p>
<p><a href="http://blog.yourtion.com/wordpress-to-jekyll.html">迁移WordPress到Jekyll</a></p>
<h3>导出WordPress数据</h3>
<p>首先，在 WordPress 后台 -&gt; 工具 -&gt; 导出。即可导出并下载包含全部文章、页面、评论、自定义栏目、分类目录和标签的 xml 文件。</p>
<h3>将xml转成Markdown</h3>
<p>使用 <code>exitwp</code> <strong>https://github.com/yourtion/exitwp</strong></p>
<p>下载后将导出的 xml 放入 <strong>wordpress-xml</strong> 目录中，运行 <code>python exitwp.py</code> 即可将 xml 转成 Markdown。
需要注意的是转换出的文件注释部分，关于Tags，Category，Date等部分需要按先前的形式进行格式调整，以便被Pelican解析。</p>
<h2>至此，我们就完成了WordPress到Pelican的迁移了～</h2>



