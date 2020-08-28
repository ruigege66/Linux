## 一、安装部署消息队列KAFKA
### 1.首先准备Zookeeper服务
- kafka是依赖于Zookeeper的，所以首先先运行Zookeeper。
- 先启动依赖，然后把安装包`kafka_2.12-2.5.0.taz`放到`/root`目录下，并解压到新建的一个目录中
```linux
[root@k8s-master ~]# cd /usr/local/zookeeper/apache-zookeeper-3.6.1-bin/bin/
[root@k8s-master bin]# ./zkServer.sh start
[root@k8s-master bin]# cd /usr/local
[root@k8s-master local]# mkdir kafka
[root@k8s-master local]# cd kafka/
[root@k8s-master kafka]# tar -zxvf /root/kafka_2.12-2.5.0.tgz -C ./
```
- 新建一个logs目录，等下要将该目录的路径配置到kafka的配置文件中
```linux
[root@k8s-master kafka_2.12-2.5.0]# mkdir logs
[root@k8s-master kafka_2.12-2.5.0]# cd config/
[root@k8s-master config]# vim server.properties 
```
- 只需修改log.dirs即可
```vim
log.dirs=/usr/local/kafka/kafka_2.12-2.5.0/logs
```
- 开始启动kafka，如果需要后台启动，则需要加上`-daemon`参数即可。
```linux
[root@k8s-master config]# cd ..
[root@k8s-master kafka_2.12-2.5.0]# ./bin/kafka-server-start.sh ./config/server.properties
```
![14.1](https://img-blog.csdnimg.cn/20200828231121872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70#pic_center)
- 接下俩我们验证一下，创建一个名为`ruigege`的`topic`,创建完成后，使用命令来列出目前已经有的`topic`列表
```linux
[root@k8s-master kafka_2.12-2.5.0]# ./bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic ruigege
[root@k8s-master kafka_2.12-2.5.0]# ./bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```
- 接下来创建一个生产者，用于在ruigege上这个topic生产消息
```linux
[root@k8s-master kafka_2.12-2.5.0]# ./bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic ruigege
[root@k8s-master kafka_2.12-2.5.0]# ./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic ruigege
```
- 我们终于部署完这些工具了，下面开始搭建一个前后端分离的开源项目
## 二、搭建一个开源项目
- 选取码云上的一个开源项目ruoyi
```linux
[root@k8s-master ~]# mkdir project
[root@k8s-master ~]# cd project/
[root@k8s-master project]# git clone https://gitee.com/y_project/RuoYi-Vue.git
```
## 三、源码：
- 搭建一个开源项目14-安装部署Kafka以及下载ruoyi.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目14-安装部署Kafka以及下载ruoyi.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![100.0](https://img-blog.csdnimg.cn/20200808233919811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)

