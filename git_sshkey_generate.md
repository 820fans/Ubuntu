搬运博客：http://blog.chinaunix.net/uid-26185912-id-3327885.html

1 如果没有安装ssh，那么使用下面的指令

sudo apt-get install ssh

2 检查SSH公钥

cd ~/.ssh

看看存不存在.ssh，如果存在的话，掠过下一步；不存在的请看下一步


3 生成SSH公钥
$ ssh-keygen -t rsa -C "your_email@youremail.com" 
# Creates a new ssh key using the provided email Generating public/private rsa key pair. 
Enter file in which to save the key (/home/you/.ssh/id_rsa):

现在你可以看到，在自己的目录下，有一个.ssh目录，说明成功了
3.1 输入github密码

Enter passphrase (empty for no passphrase): [Type a passphrase] 
Enter same passphrase again: [Type passphrase again]

这个时候输入你在github上设置的密码。
3.2 然后在.ssh中可以看到

Your identification has been saved in /home/you/.ssh/id_rsa. 
# Your public key has been saved in /home/you/.ssh/id_rsa.pub.
# The key fingerprint is: 
# 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@youremail.com

4 添加SSH公钥到github

打开github，找到账户里面添加SSH，把idrsa.pub内容复制到key里面。


5 测试是否生效

使用下面的命令测试
ssh -T git@github.com

当你看到这些内容放入时候，直接yes
The authenticity of host 'github.com (207.97.227.239)' can't be established. 
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48. 
Are you sure you want to continue connecting (yes/no)?

看到这个内容放入时候，说明就成功了。
Hi username! 
You've successfully authenticated, but GitHub does not provide shell access.

