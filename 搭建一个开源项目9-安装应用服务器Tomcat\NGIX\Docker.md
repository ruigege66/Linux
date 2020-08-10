## 一、应用服务器Tomcat安装与部署
- 将`apache-tomat-8.5.55.tar.gz`放在`\root`目录下面
- 在`/usr/local`下创建`tomcat`文件夹，并进入，解压安装包到这个目录
```Linux
[root@localhost ~]# cd /usr/local
[root@localhost local]# mkdir tomcat
[root@localhost local]# cd tomcat
[root@localhost tomcat]# tar -zxvf /root/apache-tomcat-8.5.55.tar.gz -C ./
```
- 启动tomcat
```Linux
[root@localhost tomcat]# cd apache-tomcat-8.5.55/
[root@localhost apache-tomcat-8.5.55]# cd bin
[root@localhost bin]# ./startup.sh
[root@localhost bin]# firewall-cmd --zone=public --add-port=8080/tcp --permanent
[root@localhost bin]# firewall-cmd --reload
```
![9.1](https://img-blog.csdnimg.cn/20200810200252100.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
- windows浏览器访问IP:8080即可
![9.2](https://img-blog.csdnimg.cn/20200810200957576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
- 配置快捷操作和开机启动
- 创建一个文件夹，并赋予执行权限，编辑`tomcat`文件
```Linux
[root@localhost bin]# cd /etc/rc.d/init.d/
[root@localhost init.d]# touch tomcat
[root@localhost init.d]# chmod +x tomcat
```
```vim
#!/bin/bash
#chkconfig:- 20 90
#description:tomcat
#processname:tomcat
TOMCAT_HOME=/usr/local/tomcat/apache-tomcat-8.5.55
case $1 in
        start) su root $TOMCAT_HOME/bin/startup.sh;;
        stop) su root $TOMCAT_HOME/bin/shutdown.sh;;
        *) echo "require start|stop";;

esac
```
- 后续对于Tomcat的开启和关闭只需要执行下面的命令即可
```Linux
[root@localhost init.d]# service tomcat start
[root@localhost init.d]# service tomcat stop
```
- 最后加入开机启动项
```Linux
[root@localhost tomcat]# chkconfig --add tomcat
[root@localhost tomcat]# chkconfig tomcat on
```
- 如果遇到servce命令不好用，请参见：[https://www.cnblogs.com/knownfreestyle/archive/2019/08/20/11383884.html](https://www.cnblogs.com/knownfreestyle/archive/2019/08/20/11383884.html)
> service命令用于对系统服务进行管理，比如启动（start）、停止（stop）、重启（restart）、查看状态（status）等。
service命令本身是一个shell脚本，它在/etc/init.d/目录查找指定的服务脚本，然后调用该服务脚本来完成任务。
service运行指定服务（称之为System V初始脚本）时，把大部分环境变量去掉了，只保留LANG和TERM两个环境变量，并且把当前路径置为/，也就是说是在一个可以预测的非常干净的环境中运行服务脚本。这种脚本保存在/etc/init.d目录中，它至少要支持start和stop命令。
引自：[https://www.cnblogs.com/wuheng1991/p/7064067.html](https://www.cnblogs.com/wuheng1991/p/7064067.html)

>chkconfig命令用来更新（启动或停止）和查询系统服务的运行级信息。谨记chkconfig不是立即自动禁止或激活一个服务，它只是简单的改变了符号连接。
使用语法：
chkconfig [--add][--del][--list][系统服务] 或 chkconfig [--level <等级代号>][系统服务][on/off/reset]
chkconfig 在没有参数运行时，显示用法。如果加上服务名，那么就检查这个服务是否在当前运行级启动。如果在服务名后面指 定了on，off或者reset，那么chkconfi 会改变指定服务的启动信息。on和off分别指服务被启动和停止，reset指重置服务的启动信息，无论有问题的初始化脚本指定了什么。on和off开 关，系统默认只对运行级3，4，5有效，但是reset可以对所有运行级有效。
参数用法：
–add 　增加所指定的系统服务，让chkconfig指令得以管理它，并同时在系统启动的叙述文件内增加相关数据。
–del 　删除所指定的系统服务，不再由chkconfig指令管理，并同时在系统启动的叙述文件内删除相关数据。
–level<等级代号> 　指定读系统服务要在哪一个执行等级中开启或关毕。
等级0表示：表示关机
等级1表示：单用户模式
等级2表示：无网络连接的多用户命令行模式
等级3表示：有网络连接的多用户命令行模式
等级4表示：不可用
等级5表示：带图形界面的多用户模式
等级6表示：重新启动
引自：[http://www.ttlsa.com/linux-command/linux-chkconfig-1/](http://www.ttlsa.com/linux-command/linux-chkconfig-1/)
## 二、安装web服务器NGINX
### 1.将安装包`nginx-1.17.10.tar.gz`放在`\root`下面
- 在`/usr/local/`下创建`nginx`文件夹并进入解压
```Linux
[root@localhost ~]# cd /usr/local
[root@localhost local]# mkdir nginx
[root@localhost local]# cd nginx/
[root@localhost nginx]# tar zxvf /root/nginx-1.17.10.tar.gz  -C ./
[root@localhost nginx]# cd nginx-1.17.10/
[root@localhost nginx-1.17.10]# yum -y install pcre-devel^C
```
- 预先安装额外的依赖
```Linux
[root@localhost nginx-1.17.10]# yum -y install pcre-devel
[root@localhost nginx-1.17.10]# yum -y install openssl openssl-devel
```
- 编译安装NGINX
```Linux
[root@localhost nginx-1.17.10]# ./configure
[root@localhost nginx-1.17.10]# make && make install
```
- 安装后，Nginx可执行文件位置位于
```Linux
/usr/local/nginx/sbin/ngix
```
- 启动和关闭ngix,以及修改了配置想要重新加载Ngix
```Linux
[root@localhost nginx-1.17.10]# /usr/local/nginx/sbin/nginx 
[root@localhost nginx-1.17.10]# /usr/local/nginx/sbin/nginx -s stop
[root@localhost nginx-1.17.10]# /usr/local/nginx/sbin/nginx -s reload
```
- 注意其配置文件位于`/usr/local/nginx/conf/nginx.conf`
- 打开端口防火墙之后，登录`http:192.168.1.9:80`即可
![9.3](https://img-blog.csdnimg.cn/20200810205634656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
## 三、安装DOCKER
- 直接安装docker,并启动和查看安装结果
```Linux
[root@localhost nginx-1.17.10]# yum install -y docker
[root@localhost nginx-1.17.10]# systemctl start docker.service
[root@localhost nginx-1.17.10]# docker version
```
![9.4](https://img-blog.csdnimg.cn/20200810213952384.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
- 设置开启启动以及配置DOCKER镜像下下载加速
```Linux
[root@localhost nginx-1.17.10]# systemctl enable docker.service
[root@localhost nginx-1.17.10]# docker pull mysql
```
![9.5](https://img-blog.csdnimg.cn/20200810220756702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
- 从图上我们可以看到下载速度很慢，于是我们手动配置镜像加速源，提升获取`docker`镜像速度
```Linux
[root@localhost nginx-1.17.10]# vim /etc/docker/daemon.json 
```
```vim
{
        "registry-mirrors":["http://hub-mirror.c.163.com"]
}

```
- 这里使用了网易的加速源，也可以使用阿里云、DaoCloud等资源，配置完成后，重新加载配置文件，重启docker
```Linux
[root@localhost nginx-1.17.10]# systemctl daemon-reload
[root@localhost nginx-1.17.10]# systemctl restart docker.service
```
- 这样就配置好了
> systemctl 提供了一组子命令来管理单个的 unit，其命令格式为：
systemctl [command] [unit]
command 主要有：
start：立刻启动后面接的 unit。
stop：立刻关闭后面接的 unit。
restart：立刻关闭后启动后面接的 unit，亦即执行 stop 再 start 的意思。
reload：不关闭 unit 的情况下，重新载入配置文件，让设置生效。
enable：设置下次开机时，后面接的 unit 会被启动。
disable：设置下次开机时，后面接的 unit 不会被启动。
status：目前后面接的这个 unit 的状态，会列出有没有正在执行、开机时是否启动等信息。
is-active：目前有没有正在运行中。
is-enable：开机时有没有默认要启用这个 unit。
kill ：不要被 kill 这个名字吓着了，它其实是向运行 unit 的进程发送信号。
show：列出 unit 的配置。
mask：注销 unit，注销后你就无法启动这个 unit 了。
unmask：取消对 unit 的注销。
引自：[https://blog.csdn.net/skh2015java/article/details/94012643](https://blog.csdn.net/skh2015java/article/details/94012643)
## 四、源码：
- 搭建一个开源项目8-安装RabbitMQ.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目8-安装RabbitMQ.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![100.0](https://img-blog.csdnimg.cn/20200808233919811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)

