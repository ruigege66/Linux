## 一、安装Python
- CentOS 7.4自带了一个python2.7环境
![5.1](https://imgkr2.cn-bj.ufileos.com/3add7a59-e7d3-4ad6-807e-2dbd70dcf048.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=PdQdb6jFtls8cQf2ze62dCneG6E%253D&Expires=1596639569)
- 然而我们并不想要python2,现在基本都是python3了，我们打造一个二者共存的环境
- 将压缩包`python-3.8.3.tgz`放在`/root`下面，并解压
```Linux
[root@localhost ~]# tar zxvf Python-3.8.3.tgz 
```
- 然后安装相关的依赖
```Linux
[root@localhost ~]# yum install zlib-devel bzip2-devel openssl-devel ncurses-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make
```
![5.2](https://imgkr2.cn-bj.ufileos.com/21b5d278-8a47-49ad-8702-eaa31c791fd7.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=FV%252F2JPVdJhoggN%252FduPwhNofrjkw%253D&Expires=1596640016)
- 下面编译源码并且安装，我们指定安装目录为`/usr/local/python3`
```Linux
[root@localhost ~]# cd Python-3.8.3/
[root@localhost Python-3.8.3]# ./configure prefix=/usr/local/python3
[root@localhost Python-3.8.3]# make && make install
```
- 执行上面的命令，就会自动生成目录
- 我们需要把目录`/usr/loacl/python3`中的`python3`可执行做成一份软链接，连接到`/usr/bin`下，方便后续方便调用python3
```Linux
[root@localhost Python-3.8.3]# ln -s /usr/local/python3/bin/python3 /usr/bin/python3
[root@localhost Python-3.8.3]# ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```
- 我们分别检查一下不同命令行的效果，Linux中有两种python环境了
![5.3](https://imgkr2.cn-bj.ufileos.com/22216187-071e-4b6d-b2a3-e09ff4e608e3.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=u322iMN3KtsX%252B1YjNWJ5DiGCInc%253D&Expires=1596640640)
> “./configure --prefix=路径”的作用是：编译的时候用来指定程序存放路径。\
不指定prefix，可执行文件默认放在/usr /local/bin，库文件默认放在/usr/local/lib，配置文件默认放在/usr/local/etc。其它的资源文件放在/usr /local/share。
引用：[https://zhidao.baidu.com/question/535223201.html](https://zhidao.baidu.com/question/535223201.html)

> make是编译的意思。就是把源码包编译成二进制可执行文件\
make install 就是安装的意思。

> ln命令是为某一个文件在另外一个位置建立一个同步的链接。\
-b 删除，覆盖以前建立的链接\
-d 允许超级用户制作目录的硬链接\
-f 强制执行\
-i 交互模式，文件存在则提示用户是否覆盖\
-n 把符号链接视为一般目录\
-s 软链接(符号链接)\
-v 显示详细的处理过程\
引用：[https://www.runoob.com/linux/linux-comm-ln.html](https://www.runoob.com/linux/linux-comm-ln.html)
## 二、安装MAVEN工具
- 该工具是用来项目构建以及管理工具
- 把`apache-maven-3.6.3-bin.tar.gz`包放在`/opt/maven`目录下
- 执行解压命令
```Linux
[root@localhost maven]# tar zxvf apache-maven-3.6.3-bin.tar.gz 
```
- 配置MAVEN加速镜像源，这里配置的是阿里云的，修改`/opt/maven/apache-maven-3.6.3/conf/settings.xml`
```Linux
[root@localhost maven]# vim /opt/maven/apache-maven-3.6.3/conf/settings.xml
```
- 打开vim之后，我们修改<mirrors></mirrors>这对标签
```vim
  <mirrors>
    <mirror>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <mirrorOf>central</mirrorOf>
  </mirrors>
```
- 保存退出，开始配置环境变量，修改`/etc/profile`文件，文件末尾添加如下内容
```vim
export MAVEN_HOME=/opt/maven/apache-maven-3.6.3
export PATH=$MAVEN_HOME/bin:$PATH
```
- 然后重新刷新环境变量`source /etc/profile`,并执行`mvn -v`检查是否安装好了
![5.4](https://imgkr2.cn-bj.ufileos.com/e53370cf-c9f3-49c6-bfb8-b0d64bf4bd7b.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=Fb0k%252FdSGkKpyQxdpqMvKjWMIRtY%253D&Expires=1596642635)

## 四、源码：
- 搭建一个开源项目5-安装python双环境以及Maven工程管理工具.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目5-安装python双环境以及Maven工程管理工具.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![127.59](https://static01.imgkr.com/temp/bd7c665638af480e97f18afd5062a416.jpg)
