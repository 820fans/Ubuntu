
>在Windows下，Apache的配置文件通常只有一个，就是httpd.conf。但我在Ubuntu Linux上用apt-get install apache2命令安装了Apache2后，竟然发现它的httpd.conf（位于/etc/apache2目录）是空的！进而发现Ubuntu的 Apache软件包的配置文件并不像Windows的那样简单，它把各个设置项分在了不同的配置文件中，看起来复杂，但仔细想想设计得确实很合理。

严格地说，Ubuntu的Apache（或者应该说Linux下的Apache？我不清楚其他发行版的apache软件包）的配置文件是 /etc/apache2/apache2.conf，Apache在启动时会自动读取这个文件的配置信息。而其他的一些配置文件，如 httpd.conf等，则是通过Include指令包含进来。在apache2.conf中可以找到这些Include行：
```
# Include module configuration:
Include /etc/apache2/mods-enabled/*.load
Include /etc/apache2/mods-enabled/*.conf

# Include all the user configurations:
Include /etc/apache2/httpd.conf

# Include ports listing
Include /etc/apache2/ports.conf
……
# Include generic snippets of statements
Include /etc/apache2/conf.d/

# Include the virtual host configurations:
Include /etc/apache2/sites-enabled/
```
结合注释，可以很清楚地看出每个配置文件的大体作用。当然，你完全可以把所有的设置放在apache2.conf或者httpd.conf或者任何一个配置文件中。Apache2的这种划分只是一种比较好的习惯。

安装完Apache后的最重要的一件事就是要知道Web文档根目录在什么地方，对于Ubuntu而言，默认的是/var/www。怎么知道的呢？ apache2.conf里并没有DocumentRoot项，httpd.conf又是空的，因此肯定在其他的文件中。经过搜索，发现在 /etc/apache2/sites-enabled/000-default中，里面有这样的内容：
```
NameVirtualHost *
<VirtualHost *>
ServerAdmin webmaster@localhost

DocumentRoot /var/www/
……
```
这是设置虚拟主机的，对我来说没什么意义。所以我就把apache2.conf里的Include /etc/apache2/sites-enabled/一行注释掉了，并且在httpd.conf里设置DocumentRoot为我的用户目录下的某 个目录，这样方便开发。

再看看/etc/apache2目录下的东西。刚才在apache2.conf里发现了sites-enabled目录，而在 /etc/apache2下还有一个sites-available目录，这里面是放什么的呢？其实，这里面才是真正的配置文件，而sites- enabled目录存放的只是一些指向这里的文件的符号链接，你可以用ls /etc/apache2/sites-enabled/来证实一下。所以，如果apache上配置了多个虚拟主机，每个虚拟主机的配置文件都放在 sites-available下，那么对于虚拟主机的停用、启用就非常方便了：当在sites-enabled下建立一个指向某个虚拟主机配置文件的链 接时，就启用了它；如果要关闭某个虚拟主机的话，只需删除相应的链接即可，根本不用去改配置文件。

======================================================

mods-available、mods-enabled和上面说的sites-available、sites-enabled类似，这两个目录 是存放apache功能模块的配置文件和链接的。当我用apt-get install php5安装了PHP模块后，在这两个目录里就有了php5.load、php5.conf和指向这两个文件的链接。这种目录结果对于启用、停用某个 Apache模块是非常方便的。

最后一个要说的是ports.conf，这里面设置了Apache使用的端口。如果需要调整默认的端口设置，建议编辑这个文件。或者你嫌它实在多 余，也可以先把apache2.conf中的Include /etc/apache2/ports.conf一行去掉，在httpd.conf里设置Apache端口。

ubuntu里缺省安装的目录结构很有一点不同。在ubuntu中module和 virtual host的配置都有两个目录，一个是available，一个是enabled，available目录是存放有效的内容，但不起作用，只有用ln 连到enabled过去才可以起作用。对调试使用都很方便，但是如果事先不知道，找起来也有点麻烦。

/etc/apache2/sites-available 里放的是VH的配置，但不起作用，要把文件link到 sites-enabled 目录里才行。
```
<VirtualHost *>  
        ServerName 域名  
        DocumentRoot 把rails项目里的public当根目录  
        <Directory public根目录>  
                Options ExecCGI FollowSymLinks  
                AllowOverride all  
                allow from all  
                Order allow,deny  
        </Directory>  
        ErrorLog /var/log/apache2/error-域名.log  
</VirtualHost>
```
====================================================
什么是 Virtual Hosting(虚拟主机)?
简单说就是同一台服务器可以同时处理超过一个域名(domain)。假设www.example1.net和 www.example2.net两个域名都指向同一服务器，WEB服务器又支持Virtual Hosting，那么www.example1.net和www.example2.net可以访问到同一服务器上不同的WEB空间(网站文件存放目 录)。

 
配置格式

在Apache2中，有效的站点信息都存放在/etc/apache2/sites-available/用户名(文件) 里面。 我们可以添加格式如下的信息来增加一个有效的虚拟空间，将default里的大部分东西拷贝过来就行了，记得改DocumentRoot作为默认目录，在Directory中设置路径，注意端口号不要与其他的虚拟主机重复：
```
    <VirtualHost *自定义端口>
    # 在ServerName后加上你的网站名称
    ServerName www.linyupark.com
    # 如果你想多个网站名称都取得相同的网站，可以加在ServerAlias后加上其他网站别名。
    # 别名间以空格隔开。
    ServerAlias ftp.linyupark.com mail.linyupark.com
    # 在ServerAdmin后加上网站管理员的电邮地址，方便别人有问题是可以联络网站管理员。
    ServerAdmin webmaster@linyupark.com
    # 在DocumentRoot后加上存放网站内容的目录路径(用户的个人目录)
    DocumentRoot /home/linyupark/public_html
    <Directory /home/linyupark/public_html>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride None
    Order allow,deny
    allow from all
    </Directory>
    ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
    <Directory "/usr/lib/cgi-bin">
    AllowOverride None
    Options ExecCGI -MultiViews +SymLinksIfOwnerMatch
    Allow from all
    </Directory>
    ErrorLog /home/linyupark/public_html/error.log
    # Possible values include: debug, info, notice, warn, error, crit,
    # alert, emerg.
    LogLevel warn
    CustomLog /home/linyupark/public_html/access.log combined
    ServerSignature On
    </VirtualHost>
```
    如果你的服务器有多个IP，而不同的IP又有着不一样的虚拟用户的话，可以修改成:
```
    <VirtualHost IP地址[:端口]>
    ...
    </VirtualHost>
```
启用配置

前面我们配置好的内容只是“有效”虚拟主机，真正发挥效果的话得放到 /etc/apache2/sites-enabled 文件夹下面。我们可以使用ln命令来建立一对关联文件:
```
sudo ln -s /etc/apache2/sites-available/linyupark /etc/apache2/sites-enabled/linyupark
```
检查语法，重启web服务

谨慎起见，我们在重启服务前先检查下语法：
```
sudo apache2ctl configtest
```
没有错误的话，再重启Apache
```
sudo /etc/init.d/apache2 -k restart
```
 
查看效果

主要的设置工作已经完成了，还算简单吧 ^_^。怎么看效果呢？

也简单，只要把主机上(俺用的是XP)里的Host表改改就行了。地址是:
```
WINDOWS/system32/drivers/etc
```
打开后加上一句：
```
192.168.1.22 www.linyupark.com
```
效果就是浏览器上输入www.linyupark.com就直接会去找IP 192.168.1.22 服务器收到请求，查看有没有符合的虚拟主机域名，有的话就把相应目录下的WEB文件呈现给请求用户

可能出现的错误Could not reliably determine the server's fully qualified domain name

修改/etc/apache2/httpd.conf 本文件为空，添加ServerName localhost 即可
