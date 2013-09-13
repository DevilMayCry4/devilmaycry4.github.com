---
layout: post
title: "github搭建自己的博客"
date: 2013-04-21 14:04
comments: true
categories: OSX
---
<p>登陆<a herf="https://github.com/">github</a>，创建一个个人账号。假设叫做：rubyonchina。</p>
<p><a href="https://help.github.com/articles/generating-ssh-keys">按照github的指示</a>添加ssh key</p>
<p>新建一个目录 dev.cd 到这个目录下<pre><code>cd  ~/dev/
git clone git://github.com/imathis/octopress.git rubyonchina.github.com
cd  ~/dev/rubyonchina.github.com</code></pre></p>
<p>安装ruby,rvm</p>
<p>修改默认的.rvmrc文件的内容为:</p>
<p><pre><code> rvm use 1.9.2@rails31</code></pre></p>
<p>安装相应的gem:</p>
<p><pre><code>bundle update</code></pre></p>
<p>然后生成模版文件:</p>
<p><pre>rake install</pre></p>
<p> 分发到github上。分发之前，假设你已经注册用户名为rubyonchina的github.com账号，已经创建名为rubyonchina.github.com项目。</p>
<!--more-->
<p><pre><code>cd ~/dev/rubyonchina.github.com
git remote add rubyonchina git@github.com:rubyonchina/rubyonchina.github.com.git</code></pre></p>
<p>新增一篇测试博客:</p>
<p><pre><code>rake new_post["post title"]</code></pre></p>
<p>生成静态站点</p>
<p><pre><code>rake generate</code></pre></p>
<p>在浏览器中输入:</p>
<p><pre><code>127.0.0.1:4000</code></pre></p>
<p>配置octopress与github的连接</p>
<p><pre><code>rake setup_github_pages</code></pre></p>
<p>按照提示填入你的github项目的网址,比如比例是:</p>
<p><pre><code> git@github.com:rubyonchina/rubyonchina.github.com.git </code></pre></p>
<p>分发到github上:</p>
<p><pre><code>rake deploy</code></pre></p>
<p> 第一次运行时，会询问是否建立对github的授权，输入：yes。然后将站点更新的内容推送到github上。从github上pull过数据就也可能不出现.如果没出现后面push的可以不输.</p>
<p><pre><code>git push -u rubyonchina master</code></pre></p>
<p>OK!现在你拥有自己的站点了.</p>