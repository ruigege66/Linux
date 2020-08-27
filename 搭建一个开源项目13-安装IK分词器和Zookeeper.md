## 一、安装IK分词器
- 下载ik分词器插件
```linux
wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.4.2/elasticsearch-analysis-ik-
```
- 使用linux下载会很慢，于是我自己去github上已经提前下载好了，下面开始安装
```linux
[root@k8s-master ~]# mkdir /opt/elasticsearch/elasticsearch-6.4.2/plugins/elasticsearch-analysis-ik-6.4.2
[root@k8s-node-1 ~]# cd /opt/elasticsearch/elasticsearch-6.4.2/plugins/elasticsearch-analysis-ik-6.4.2
[root@k8s-node-1 elasticsearch-analysis-ik-6.4.2]# unzip elasticsearch-analysis-ik-6.4.2.tar.gz 
```
- 解压即为安装好了IK分词器，最后重启elasticsearch集群即可
## 二、ZOOKEEPER安装部署
- 将安装包解压缩`apache-zookeeper-3.6.1-bin.tar.gz`，并将其放在`/root`目录下。
```linux
[root@k8s-master ~]# cd /usr/local
[root@k8s-master local]# mkdir zookeeper
[root@k8s-master local]# cd zookeeper/
[root@k8s-master zookeeper]# tar -zxvf /root/apache-zookeeper-3.6.1-bin.tar.gz -C ./
[root@k8s-master zookeeper]# cd apache-zookeeper-3.6.1-bin/
[root@k8s-master apache-zookeeper-3.6.1-bin]# mkdir data
```
- 我们需要将data目录地址配置到ZooKeeper的配置文件中
```linux
[root@k8s-master apache-zookeeper-3.6.1-bin]# cd conf
[root@k8s-master conf]# cp zoo_sample.cfg zoo.cfg
[root@k8s-master conf]# vim zoo.cfg 
```
- 修改配置文件，将`datadir`修改为`data`目录
![13.1](https://img-blog.csdnimg.cn/20200827235528285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70#pic_center)
- 启动Zookeeper,并检查状态
```linux
[root@k8s-master apache-zookeeper-3.6.1-bin]# ./bin/zkServer.sh start
[root@k8s-master apache-zookeeper-3.6.1-bin]# ./bin/zkServer.sh status
```
![13.2](https://img-blog.csdnimg.cn/20200827235852592.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70#pic_center)
- 从上面可以看出来绑定端口2181
- 接下来配置环境变量以及设置开机启动
```linux
[root@k8s-master ~]# vim /etc/profile
## 下面是在配置文中末尾加上这两行
export ZOOKEEPER_HOME=/usr/local/zookeeper/apache-zookeeper-3.6.1-bin
export PATH=$PATH:$ZOOKEEPER_HOME/bin
###
[root@k8s-master ~]# source /etc/profile
[root@k8s-master ~]# cd /etc/rc.d/init.d
[root@k8s-master init.d]# touch zookeeper
[root@k8s-master init.d]# chmod +x zookeeper 
[root@k8s-master init.d]# vim zookeeper
###添加下面的内容
#!/bin/bash
#chkconfig:- 20 90
#description:zookeeper
#processname:zookeeper
ZOOKEEPER_HOME=/usr/local/zookeeper/apache-zookeeper-3.6.1-bin
export JAVA_HOME=/usr/local/java/jdk1.8.0_161
case $1 in
        start) su root $ZOOKEEPER_HOME/bin/zkServer.sh start;;
        stop) su root $ZOOKEEPER_HOME/bin/zkServer.sh stop;;
        status) su root $ZOOKEEPER_HOME/bin/zkServer.sh status;;
        restart) su root $ZOOKEEPER_HOME/bin/zkServer.sh restart;;
        *) echo "require start|stop|status|restart";;
esac
###
[root@k8s-master init.d]# chkconfig --add zookeeper
[root@k8s-master init.d]# chkconfig zookeeper on
```
## 三、源码：
- 搭建一个开源项目13-安装IK分词器和Zookeeper.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目13-安装IK分词器和Zookeeper.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![100.0](https://img-blog.csdnimg.cn/20200808233919811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)

