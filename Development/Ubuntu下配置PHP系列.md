## 安装Apache, MySQL, PHP7, PhpMyAdmin

- 安装之前更新系统
- 安装apache2

```
;安装apache2
sudo apt-get install apache2
;安装php
sudo apt install php
sudo apt-get install libapache2-mod-php
;安装mysql
sudo apt install mysql-server php7.0-mysql
sudo apt-get install mysql-client
mysql_secure_installation
;安装phpmyadmin
sudo apt-get install phpmyadmin
sudo apt-get install php-mbstring
sudo apt-get install php-gettext
```
安装时选择自动配置数据库，输入数据库root账号的密码<br> 
如果不安装以上两个php软件包，则会报错或者白屏，提示找不到/usr/share/php/php-gettext/gettext.inc之类的错误
```
;建立www下的phpmyadmin软链接
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin

;配置phpmyadmin
sudo gedit /etc/php/7.0/apache2/php.ini
display_errors = On(显示错误日志，出现两次，都要改，不然无效)
extension=php_mbstring.dll (开启mbstring)

sudo /etc/init.d/apache2 restart
```
默认是配置在localhost下，var/www/是网站根目录<br>
localhost/phpmyadmin可以进入管理数据库

### 进入phpmyadmin 报404
```
sudo -H gedit /etc/apache2/apache2.conf
```
Then add the following line to the end of the file:
```
Include /etc/phpmyadmin/apache.conf
```
Then restart apache:
```
/etc/init.d/apache2 restart
```
