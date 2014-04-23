title: OpenFire
date: 2014-04-23 16:17:59
tags: IOS
---
先到<a href='http://dev.mysql.com/downloads/mysql'>http://dev.mysql.com/downloads/mysql</a>下载对应的mysql版本。
安装并启动mysql、
<p>二、打开终端，定义mysql别名</p>
<p>输入alias命令</p>
<pre>alias mysql=/usr/local/mysql/bin/mysql</pre>
<p>回车，再输入</p>
<pre>alias mysqladmin=/usr/local/mysql/bin/mysqladmin</pre>
<p>三、设置mysql root帐号的密码</p>
<pre>mysqladmin -u root password 初始密码</pre>

<p>2.如果设置完密码后，需要修改，执行命令</p>
<pre>mysqladmin -u root -p  password 最新密码</pre>

<p>接着会提示输入密码，此时输入旧密码，回车</p>
<p>&nbsp;四、连接数据库</p>
<pre>mysql -u root -p</pre>

<p>然后提示输入密码，输入三中设置的初始密码</p>
<p>2.如果登陆远程主机上的mysql数据库</p>
<pre>mysql -h 主机地址 -u 用户名 -p 用户密码</pre>

<p>&nbsp;</p>
<p>五、执行常用的mysql数据库操作</p>
<p>注意：以下操作都发现在，连接数据库之后，进入mysql环境，之后执行的命令都必须带有分号&ldquo;;&rdquo;</p>
<p>首先，以root权限连接mysql</p>
<pre>mysql -u root -p</pre>

<p>然后，输入root的密码</p>
 
<p>1、增加新用户</p>
<p>格式如下：</p>
<pre>grant 操作权限 on 数据库.* to 用户名@登陆主机地址 identified by <span style="color: #800000;">'</span><span style="color: #800000;">密码</span><span style="color: #800000;">';</span></pre>

<p>意思是：授予，某主机上的某用户（附带该用户的登陆密码）在某数据库上，执行某些操作的权限</p>
<p>(1)比如：任意主机上("%")，用户（用户名：test1，密码：adc）在所有数据库上，执行任意操作的权限（很危险）</p>
<pre>grant all privileges on *.* to test1<span style="color: #800000;">@"</span><span style="color: #800000;">%</span><span style="color: #800000;">"</span> identified by <span style="color: #800000;">"</span><span style="color: #800000;">abc</span><span style="color: #800000;">"</span>;</pre>

<p>其中all privileges表示查询，插入，修改，删除的权限：select,insert,update,delete</p>
<p>以上命令等价于：</p>
<pre>grant <span style="color: #0000ff;">select</span>,insert,update,delete on *.* to test1<span style="color: #800000;">@"</span><span style="color: #800000;">%</span><span style="color: #800000;">"</span> identified by <span style="color: #800000;">"</span><span style="color: #800000;">abc</span><span style="color: #800000;">"</span>;</pre>

<p>然后刷新权限</p>
<pre>flush privileges;</pre>

<p>&nbsp;(2)比如：授权本地主机上的用户操作数据库的权限</p>
<p><span style="color: #ff0000;">创建数据库</span>(比如：openfire)</p>
<pre>create database openfire;</pre>

<p>授予本地主机用户（用户名：test2，密码：123）访问数据库(数据库名称：openfire)的操作权限</p>
<pre>grant all privileges on openfire.* to test2@localhost identified by <span style="color: #800000;">"</span><span style="color: #800000;">123</span><span style="color: #800000;">"</span>;</pre>

<pre>flush privileges;</pre>

<p>&nbsp;之后，就可以用新的用户，访问openfire数据库了</p>
<p>2.更新指定帐户的密码（用户名：test1，新密码：1234）</p>
<pre>update mysql.user set password=password(<span style="color: #800000;">'</span><span style="color: #800000;">1234</span><span style="color: #800000;">'</span>) where User=<span style="color: #800000;">"</span><span style="color: #800000;">test1</span><span style="color: #800000;">"</span> and Host=<span style="color: #800000;">"</span><span style="color: #800000;">localhost</span><span style="color: #800000;">"</span>;</pre>

<p>&nbsp;3.删除用户</p>
<p>先使用mysql数据库</p>
<pre>use mysql;</pre>

<p>删除mysql数据库中user表中的某个本地用户（test7）</p>
<pre>delete from user where User=<span style="color: #800000;">"</span><span style="color: #800000;">test7</span><span style="color: #800000;">"</span> and Host=<span style="color: #800000;">"</span><span style="color: #800000;">localhost</span><span style="color: #800000;">"</span>;</pre>

<p>&nbsp;4.显示命令</p>
<p>（1）显示所有数据库列表</p>
<pre>show databases;</pre>

<p>初始化只有两个数据库，mysql和test</p>
<p><span>注意：MYSQL的系统信息都存储在mysql库中，比如：修改密码和新增用户，实际上就是用这个库进行操作</span></p>
<p><span>（2）打开某个数据库(比如数据库：openfire)</span></p>
<pre>use openfire;</pre>

<p>（3）显示本库中的所有表</p>
<pre>show tables;</pre>

<p>（4）显示某表（table1）的结构</p>
<pre>describe table1;</pre>

<p>（5）建库</p>
<pre>create database 库名;</pre>

<p>（6）建表</p>
<pre><span style="color: #000000;">use 库名；
create table 表名 (字段设定列表);</span></pre>

<p>（7）删库</p>
<pre>drop database 库名;</pre>

<p>（8）删表</p>
<pre>drop table 表名;</pre>

<p>（9）将表中的记录清空</p>
<pre>delete from 表名;</pre>

<p>（10）显示表中的记录</p>
<pre><span style="color: #0000ff;">select</span> * from 表名;</pre>

<p>六、退出mysql</p>
<pre>exit</pre>

<p>&nbsp;七、启动和停止MySQL<span style="font-size: 14px; line-height: 1.5;">&nbsp;</span></p>
<p>启动</p>
<pre>/usr/local/mysql/share/mysql.server start</pre>

<p>停止</p>
<pre>/usr/local/mysql/bin/mysqladmin -u root -p shutdown</pre>

<p>输入root密码</p>
<p><strong>安装openfire</strong></p>
<p>到<a href='http://www.igniterealtime.org/downloads/index.jsp'>http://www.igniterealtime.org/downloads/index.jsp</a>下载对应的openfire安装包</p>
<p>设置主机的访问ip地址</p>
![](/img/openfire1.png)  
<p>数据库url设置为:<strong>jdbc:mysql://localhost:3306/openfire?useUnicode=true&characterEncoding=UTF-8&characterSetResults=UTF-8</strong></p>
<p><strong>导入openfire数据库表</strong></p>
<p>先修改读取权限sudo chmod 777 /usr/local/openfire</p>
<p>&lt;2&gt;在终端中，登陆MySQL</p>
<pre>mysql -u root -p</pre>

<p>然后输入数据库的root密码</p>
<p>&lt;3&gt;创建数据库openfire</p>
<pre>create database openfire;</pre>

<p>&lt;4&gt;导入openfire资源文件夹<span>&nbsp;</span><tt>resources/database下的数据表</tt>在导入前修改/usr/local/openfire/resources/database/openfire_mysql.sql中创建ofRoster的语句，把
<pre>jid       VARCHAR(1024)   NOT NULL</pre>中的1024改为767</p>
<pre>use openfire;</pre>
<pre>source /usr/local/openfire/resources/database/openfire_mysql.sql</pre>

<p>&nbsp;在终端出现一排导入过程</p>
<p>&nbsp;&lt;5&gt;刷新权限</p>
<pre>flush privileges;</pre>

<p>&lt;6&gt;退出MySQL</p>
<pre>exit</pre>

<p>（4）用户名和密码</p>
<p>这里的用户名密码，是访问MySQL数据库时使用的帐号：root，和安装MySQL设置的root密码</p>
<p>5.特性设置</p>
<p>如果不打算使用LDAP，则保持默认设置</p>
![](/img/openfire2.png)  
<p>&nbsp;6.设置openfire服务器管理员的帐号和密码</p>
<p>可以随便填写一个管理员邮箱，输入要设置的密码</p>
<p>完成注册</p>
<p>&nbsp;</p>
<p><span style="font-size: 14px; line-height: 1.5;">7.登陆管理控制台</span></p>
<p>&nbsp;</p>
<p>默认的管理员帐号是&ldquo;admin&rdquo;，默认管理员密码&ldquo;admin&rdquo;，如果上面设置了新密码，则管理员密码是新密码</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>如果想去掉默认的admin帐号，并自定义，需要如下操作</p>
<p>&nbsp;</p>
<p>（1）在终端中，登陆具体的数据库（openfire）</p>
<p>&nbsp;</p>
<pre>mysql -u root -p openfire</pre>

<p>&nbsp;</p>
<p>然后输入数据库的root密码</p>
<p>&nbsp;</p>
<p>（2）删除表&ldquo;ofUser&rdquo;中的admin帐户</p>
<p>&nbsp;</p>
<pre>delete from ofUser where username=<span>'</span><span>admin</span><span>'</span>;</pre>

<p>&nbsp;</p>
<p>（3）创建自定义管理员（用户名：user1，密码：123）</p>
<p>&nbsp;</p>
<pre>INSERT INTO ofUser (username, plainPassword, encryptedPassword, name, email, creationDate, modificationDate) VALUES (<span>'</span><span>user1</span><span>'</span>,<span>'</span><span>123',</span><span>'</span><span>123</span><span>'</span><span>,</span><span>'</span>Administrator<span>'</span><span>,</span><span>'</span>user1@sunyard.com<span>'</span><span>,</span><span>'</span><span>0</span><span>'</span><span>,</span><span>'</span><span>0</span><span>'</span><span>);</span></pre>

<p>&nbsp; 注意：如果重设了用户名，必须重启openfire服务器</p>
<p>三、卸载openfire</p>
<p>1.停止服务</p>
<p>在系统偏好设置的其他里，打开openfire偏好设置</p>
<p>点击Stop Openfire按钮，停止服务</p>
<p>2.删除文件</p>
<p>打开终端，输入以下命令</p>
<pre><span style="color: #0000ff;">sudo</span> <span style="color: #0000ff;">rm</span> -rf /Library/PreferencePanes/Openfire.prefPane</pre>

<pre><span style="color: #0000ff;">sudo</span> <span style="color: #0000ff;">rm</span> -rf /usr/local/openfire</pre>

<pre><span style="color: #0000ff;">sudo</span> <span style="color: #0000ff;">rm</span> -rf /Library/LaunchDaemons/org.jivesoftware.openfire.plist</pre>

<p>其中第一条命令之后，需要输入本机管理员密码</p><div id="MySignature">