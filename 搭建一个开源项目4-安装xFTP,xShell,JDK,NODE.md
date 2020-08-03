## 一、安装xFtp和xShell
- 下载地址：[https://www.netsarang.com/zh/free-for-home-school/](https://www.netsarang.com/zh/free-for-home-school/)
- 由于我们是非商用途，可以直接下载这两款软件，xShell用于远程登录Linux，xFtp用于windows与Linux文件传输。
- 安装好软件之后我们启动传输工具
![4.1](https://imgkr2.cn-bj.ufileos.com/d1a1206c-8229-49fe-9ca5-a34ca907ff05.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=ETbsichYZcWoHGHqGLtBqK6Tuz4%253D&Expires=1596554236)

## 二、安装Oracle JDK
- 将`jdk-8u161-linux-x64.tar.gz`放到`\root`目录下
- 先卸载已经有的OPENJDK
```Linux
rpm -qa | grep java
```
![4.2](https://imgkr2.cn-bj.ufileos.com/1016c3df-c2bc-4c3b-96a9-0377ed0d175c.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=KOiv0%252BFdToaTZ6hStlA1RsvyyaA%253D&Expires=1596554531)
- 接下来查询出来的包全部都删除掉
```Linux
yum -y remove java-1.7.0-openjdk-devel-1.7.0.251-2.6.21.1.el7.x86_64
yum -y remove java-1.6.0-openjdk-devel-1.6.0.41-1.13.13.1.el7_3.x86_64
yum -y remove java-1.8.0-openjdk-devel-1.8.0.242.b08-1.el7.x86_64
yum -y remove java-1.8.0-openjdk-headless-1.8.0.242.b08-1.el7.x86_64
yum -y remove java-1.7.0-openjdk-headless-1.7.0.251-2.6.21.1.el7.x86_64
yum -y remove java-1.6.0-openjdk-1.6.0.41-1.13.13.1.el7_3.x86_64
```
> RPM共有10种基本的模式：它们是安装、查询、验证、删除等。\
安装模式：rpm –i\
查询模式：rpm –q\
验证模式：rpm –V或–verify\
删除模式：rpm –e\
例如：列出所有被安装的rpm package\
rpm -qa\
RPM是RedHat Package Manger（RedHat软件管理工具),是一种用于打包及安装工具\
grep(global search rgular expression(RE) and print out the line):是一种强大的文本搜索工具 

- 接下来解压我们刚才传好的包
```Linux
[root@localhost ~]# cd /usr/local
[root@localhost local]# mkdir java
[root@localhost local]# cd java
[root@localhost java]# tar -zxvf jdk-8u161-linux-x64.tar.gz -C ./
[root@localhost java]# tar -zxvf /root/jdk-8u161-linux-x64.tar.gz -C ./
```
- 我们将tar包解压到了/usr/local/java中
- 下面我们配置环境变量
```Linux
[root@localhost java]# vim /etc/profile
```
- 进入到vim中，在尾行添加
```vim
JAVA_HOME=/usr/local/java/jdk1.8.0_161
CLASSPATH=$JAVA_HOME/lib/
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME CLASSPATH
```
- 执行下面重新生效,并验证结果
```Linux
[root@localhost java]# source /etc/profile
```
![4.3](https://imgkr2.cn-bj.ufileos.com/f493c1d9-5b18-4d1a-83c5-91f4799ee4fe.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=lEsji8nwFwjYfSRh5qq9L810WUI%253D&Expires=1596556261)
- JDK就安装好了
### 三、NODE环境安装
- 将安装包`node-v12.16.3-linux-x64.tar.xz`直接放在`/root`目录下，并且解压到`/usr/local/node`中
```Linux
[root@localhost local]# mkdir node
[root@localhost local]# cd node
[root@localhost node]# tar -xJvf /root/node-v12.16.3-linux-x64.tar.xz -C ./
```
> *.tar 用 tar –xvf 解压\
*.gz 用 gzip -d或者gunzip 解压\
*.tar.gz和*.tgz 用 tar –xzf 解压\
*.bz2 用 bzip2 -d或者用bunzip2 解压\
*.tar.bz2用tar –xjf 解压\
*.Z 用 uncompress 解压\
*.tar.Z 用tar –xZf 解压\
*.rar 用 unrar e解压\
*.zip 用 unzip 解压\
引自[https://www.cnblogs.com/newcaoguo/p/5896975.html](https://www.cnblogs.com/newcaoguo/p/5896975.html)
- 接下来配置环境变量
```Linux
[root@localhost node]# vim ~/.bash_profile
```
- 在文件末尾添加如下
```vim
#Nodejs
export PATH=/usr/local/node/node-v12.16.3-linux-x64/bin:$PATH
```
- 重启环境变量，并检查安装结果
```Linux
[root@localhost node]# source ~/.bash_profile
[root@localhost node]# npm version
[root@localhost node]# npm version
[root@localhost node]# npx -v
```
![4.4](https://imgkr2.cn-bj.ufileos.com/c919a0a5-cde4-42a4-965c-2db722a8ec30.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=HKg%252BUBm7r11ubaVVuA1R09JOan0%253D&Expires=1596557229)

## 四、源码：
- 搭建一个开源项目4-安装xFTP,xShell,JDK,NODE.md
- 
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![127.59](https://static01.imgkr.com/temp/bd7c665638af480e97f18afd5062a416.jpg)
