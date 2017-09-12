>16.10安装之后应该做的事情，主体部分参考：
http://blog.csdn.net/lj402159806/article/details/54695651

### 1. chromium浏览器
chrome精简版，登录chrome账户之后会将chrome里面的配置都同步过来；
参考 http://chromium.woolyss.com/  [需要翻墙]
```bash
sudo add-apt-repository ppa:canonical-chromium-builds/stage
sudo apt-get update
sudo apt-get install chromium-browser
```
### 2. 安装Java 
还不错，用idea之前装的，没有啥坑，参考 http://blog.csdn.net/lj402159806/article/details/54695651
``` 
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update 
```
安装oracle-java-installer
```
# JDK8
sudo apt-get install oracle-java8-installer
```
安装器会提示你同意 oracle 的服务条款,选择 ok
然后选择yes 即可。设置系统默认jdk(也可用于同时安装两个版本,实现两者的切换)
```
# JDK8
sudo update-java-alternatives -s java-8-oracle
# 测试jdk是否安装成功:
java -version
javac -version
```

### 3. 安装systemback  => 无敌的备份工具，非常好用
链接 : http://www.linuxidc.com/Linux/2015-09/122560.htm
```
sudo add-apt-repository ppa:nemh/systemback
sudo apt-get update
sudo apt-get install systemback
# 卸载命令：
sudo apt-get remove systemback
```

### 4. 倒腾QQ  => 放弃ubuntu下使用qq
http://www.mintos.org/review/linux-qq.html

### 5. anaconda安装完应该做的
```
conda install libgcc # 用于读取libsvm.so.2，可选
conda install gensim # 用于使用word2vec
conda install theano
conda install keara
```
#### 5.1 libsvm安装配置
```
libsvm各种准备，在下载完成之后，cd根目录
根目录 make
根目录/python make
pycharm 里面需要把libsvm.so.2放到lib/python
sudo apt-get install gnuplot-x11 #用于gnuplot画图
安装完成之后需要使用which gnuplot 查看位置，更新grid.py
```
### 6. shadowsocks科学上网
安装 shadowsocks GUI 客户端
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```
### 7. 安装wps
去官网http://linux.wps.cn/ 下载最新的alpha版本deb
#### 7.1 安装版本
##### 7.1.1 解决依赖
由于需要32位的支持，输入
```
sudo apt-get install ia32-libs -------------------- 报错
```
于是：
```
sudo apt-get install lib32ncurses5
sudo apt-get install lib32z1
```
然后发现还是缺少依赖libpng12-0，于是从以下链接下载：
https://packages.debian.org/zh-cn/wheezy/amd64/libpng12-0/download
```
#sudo dpkg -i libpng12-0_1.2.49-1+deb7u2_amd64.deb
#之后就可以正常安装了，缺少字体的问题，按照这个blog解决：
http://blog.csdn.net/star_xiong/article/details/47148413
#之后，wps完美安装
```
##### 7.1.2 解决字体问题
1. 下载该字体后，解压到 /usr/share/fonts/  目录下，解压出来的目录为wps_symbol_fonts
注意，wps_symbol_fonts目录要有可读可执行权限，没有的话需要手动修改
权限设置,执行命令如下
```
cd /usr/share/fonts/
chmod 755 wps_symbol_fonts
cd /usr/share/fonts/wps_symbol_fonts 
chmod 644 *
```
2. 生成缓存配置信息,进入字体目录
```
cd /usr/share/fonts/wps_symbol_fonts
生成
mkfontdir
mkfontscale
fc-cache
```
#### 7.2 绿色版本
...好像说绿色版本除了字体不会报错，wtf...没试过

### 8. PDF阅读器
PDF 阅读器   ==> 超级好用
```
sudo apt-get install okular
```
### 9. Ubuntu下FTP服务器  ==> 和小米“远程管理”无缝衔接
不过我当时是下载了一个zip文件到本地用的
```
sudo add-apt-repository ppa:n-muench/programs-ppa
sudo apt-get update
sudo apt-get install filezilla
```
### 10. 安装截图工具 Shutter  ==>  完全替代QQ截图
```
sudo apt-get install shutter
# 设置快捷键
shutter -s
# shutter -a # 截屏活动窗口
```
### 11. 管理ubuntu剪切板
```
sudo apt-get install parcellite
```
通过管理首选项的trim newlines ， 就可以在复制pdf文件的时候，自动去除回车。

### 12. 标题栏显示网速
标题栏实时显示网速
http://blog.csdn.net/tecn14/article/details/24489031
```
sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor  
sudo apt-get update  
sudo apt-get install indicator-sysmonitor 
```

### 13. 格式化U盘
>但是我拿这个刻live盘之后不能正常用<br>
```
sudo fdisk -l
sudo fdisk /dev/sdc
# http://www.linuxidc.com/Linux/2013-05/85115.htm
# 刻盘 
# dd命令做usb启动盘十分方便,只须:
sudo dd if=xxx.iso of=/dev/sdb      # 但是我拿这个刻live盘之后不能正常用
```

### 14. 安装Photoshop替代产品GIMP
```
sudo add-apt-repository ppa:otto-kesselgulasch/gimp
sudo apt-get update
sudo apt-get install gimp
```