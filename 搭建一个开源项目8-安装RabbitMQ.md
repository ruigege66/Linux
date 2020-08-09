
## 一、消息队列RabbitMQ安装部署
### 1.首先安装Erlang环境
- 这是RabbitMQ的依赖，所以首先要安装它，执行下面命令来安装对应的`yum repo`
```Linux
[root@localhost ~]# curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash
```
![8.1](https://img-blog.csdnimg.cn/20200809132807991.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
- 接下来执行下面的命令安装`erlang`环境
```Linux
[root@localhost ~]# yum install erlang.x86_64 
[root@localhost ~]# yum install erlang-22.3.3-1.e17.x86_64

```


> curl是一个利用URL规则在命令行下工作的文件传输工具，可以说是一款很强大的http命令行工具。它支持文件的上传和下载，是综合传输工具，但按传统，习惯称url为下载工具。
> 参数：-A/--user-agent <string>              设置用户代理发送给服务器\
-b/--cookie <name=string/file>    cookie字符串或文件读取位置\
-c/--cookie-jar <file>                    操作结束后把cookie写入到这个文件中\
-C/--continue-at <offset>            断点续转\
-D/--dump-header <file>              把header信息写入到该文件中\
-e/--referer                                  来源网址\
-f/--fail                                          连接失败时不显示http错误\
-o/--output                                  把输出写到该文件中\
-O/--remote-name                      把输出写到该文件中，保留远程文件的文件名\
-r/--range <range>                      检索来自HTTP/1.1或FTP服务器字节范围\
-s/--silent                                    静音模式。不输出任何东西\
-T/--upload-file <file>                  上传文件\
-u/--user <user[:password]>      设置服务器的用户和密码\
-w/--write-out [format]                什么输出完成后\
-x/--proxy <host[:port]>              在给定的端口上使用HTTP代理\
-#/--progress-bar                        进度条显示当前的传送状态\
来源：[https://www.cnblogs.com/duhuo/p/5695256.html](https://www.cnblogs.com/duhuo/p/5695256.html)

### 2.下面开始安装`RabbitMQ`
- 先安装其对应的`yum rep`，然后后一条命令进行安装rabbitmq包
```Linux
[root@localhost ~]# curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
[root@localhost ~]# yum install rabbitmq-server.noarch 
```
![8.3](https://img-blog.csdnimg.cn/20200809144407802.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)

> 在linux中，&和&&,|和||介绍如下：
&  表示任务在后台执行，如要在后台运行redis-server,则有  redis-server &
&& 表示前一条命令执行成功时，才执行后一条命令 ，如 echo '1‘ && echo '2'    
| 表示管道，上一条命令的输出，作为下一条命令参数，如 echo 'yes' | wc -l
|| 表示上一条命令执行失败后，才执行下一条命令，如 cat nofile || echo "fail"
引自：[https://blog.csdn.net/chinabestchina/article/details/72686002 ](https://blog.csdn.net/chinabestchina/article/details/72686002)

> sudo bash 表示以root的身份运行bash
### 3.设置RabbitMQ开机启动,并启动该服务
```Linux
[root@localhost ~]# chkconfig rabbitmq-server on
[root@localhost ~]# systemctl start rabbitmq-server.service 
```
### 4.开启web可视化管理插件
```Linux
[root@localhost ~]# rabbitmq-plugins enable rabbitmq_management
```
![8.4](https://img-blog.csdnimg.cn/20200809144827627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
### 5.访问可视化管理界面
- 地址:IP地址：15672
- 我们在windows上访问它的时候，显示报错，ping了一下linux的地址，显示成功，这说明连接没有问题，我们没有开启linux的防火墙的原因，因此我们开启防火墙即可
```Linux
[root@localhost ~]# firewall-cmd --zone=public --add-port=15672/tcp --permanent
[root@localhost ~]# firewall-cmd --reload
```
- 再放文件可以了，注意有一个参数permanent要加上，否则，我们重启Linux原来的配置就会失效
- 参考于大佬的博客：[https://www.cnblogs.com/zipxzf/p/11249846.html](https://www.cnblogs.com/zipxzf/p/11249846.html)
![8.5](https://img-blog.csdnimg.cn/20200809150503808.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
### 4.后台添加一个用户和密码
```Linux
[root@localhost ~]# rabbitmqctl add_user dongqianrui 密码
[root@localhost ~]# rabbitmqctl set_user_tags dongqianrui administrator
```
- 我们再web登录
![8.6](https://img-blog.csdnimg.cn/20200809150807155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
- 大功告成
## 四、源码：
- 搭建一个开源项目8-安装RabbitMQ.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目8-安装RabbitMQ.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![100.0](https://img-blog.csdnimg.cn/20200808233919811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)

