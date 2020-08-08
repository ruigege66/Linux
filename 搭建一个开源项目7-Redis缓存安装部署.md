## 一、安装Redis缓存
### 1.将包放到`root`目录下面
### 2.在`/usr/local/`下面创建`redis`文件夹并进入解压到这个文件夹
```Linux
[root@localhost ~]# cd /usr/local
[root@localhost local]# mkdir redis
[root@localhost local]# cd redis/
[root@localhost redis]# tar zxvf /root/redis-5.0.8.tar.gz -C ./
```
### 3.编译并安装
```Linux
[root@localhost redis]# cd redis-5.0.8/
[root@localhost redis-5.0.8]# make && make install
```
### 4.将Redis安装为系统服务并且后台启动
- 进入到`utils`目录，并执行以下脚本
```Linux
[root@localhost redis-5.0.8]# cd utils/
[root@localhost utils]# ./install_server.sh
```
- 然后一直回车就可以了（我们按照默认的配置安装）
![7.1](https://img-blog.csdnimg.cn/20200808230913172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
### 5.查看Redis服务启动情况
- 直接执行下面的命令来查看启动的结果
```Linux
[root@localhost utils]# systemctl status redis_6379.service 
```
- 如果出现失败的情况如下：
![7.2](https://img-blog.csdnimg.cn/20200808231607370.png)
- 那么执行下面的语句，直到出现成功图片
```Linux
[root@localhost utils]# systemctl stop redis_6379
[root@localhost utils]# systemctl start redis_6379
[root@localhost utils]# systemctl status redis_6379.service 
```
![7.3](https://img-blog.csdnimg.cn/20200808231802920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
### 6.启动Redis客户端并测试
- 启动自带的`redis-cli`客户端，测试通过
```Linux
[root@localhost utils]# redis-cli
127.0.0.1:6379> 
127.0.0.1:6379> set foo bar
OK
127.0.0.1:6379> get foo
"bar"
127.0.0.1:6379> 
```
- 但是此时还只能本地访问，无法完成远程连接，因此还要其他设置
### 7.设置允许远程连接
- 编辑`redis`配置文件
```Linux
vim /etc/redis/6379.conf
```
- 修改127.0.0.1为0.0.0.0
![7.4](https://img-blog.csdnimg.cn/20200808232635574.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
- 然后重启redis
```Linux
[root@localhost utils]# systemctl restart redis_6379.service 
```
### 8.设置访问密码
- 编辑redis配置文件
```Linux
[root@localhost utils]# vim /etc/redis/6379.conf
```
找到#requirepass foobared，去掉#号，并且修改密码
![7.5](https://img-blog.csdnimg.cn/20200808233607525.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
- 保存后重启服务
```Linux
[root@localhost utils]# systemctl restart redis_6379.service 
```
![7.6](https://img-blog.csdnimg.cn/20200808233833360.png)
- 可以了
## 四、源码：
- 搭建一个开源项目7-Redis缓存安装部署.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目7-Redis缓存安装部署.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![100.0](https://img-blog.csdnimg.cn/20200808233919811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
