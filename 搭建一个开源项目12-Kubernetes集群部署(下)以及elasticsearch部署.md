
## 一、配置Kubectl
- 在Master上执行下面的命令来配置Kubectl
```linux
[root@k8s-master ~]# echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> /etc/profile
[root@k8s-master ~]# source /etc/profile
[root@k8s-master ~]# echo $KUBECONFIG
```
## 二、安装Pod网络
- 安装Pod网络是Pod之间进行通信的必要条件，k8s支持众多网络方案，这里我们依然选用经典的flannel方案
- 首先是设置系统参数，主从都设置
```linux
[root@k8s-master bridge]# sysctl net.bridge.bridge-nf-call-iptables=1
```
- 然后在Master节点上执行如下命令：
```linux
kubectl apply -f kube-flannel.yaml
```
- `kube-flannel.yam`下载地址：[https://github.com/hansonwang99/JavaCollection/edit/master/docs/kubernetes/kube-flannel.yaml](https://github.com/hansonwang99/JavaCollection/edit/master/docs/kubernetes/kube-flannel.yaml)
- 一旦Pod网络安装完成，可以执行下面的命令来进行检查CoreDNS Pod此刻是否正常运行起来的，一旦正常运行起来就可以执行后面步骤：
```linux
kubectl get pods --all-namespaces -o wide
```
## 三、安装ElasticSearch集群部署
- 推荐一个开源镜像仓[https://mirrors.huaweicloud.com/](https://mirrors.huaweicloud.com/),华为的开源镜像仓，大家都可以进
- 节点准备，这里准备了两个节点
- 节点一：192.168.1.9;节点二：192.168.1.8
- 下载elasticsearch，从开源镜像仓，然后放到`/opt/elasticsearch`目录中
```linux
[root@k8s-master ~]# mkdir /opt/elasticsearch
[root@k8s-master ~]# cd /opt/elasticsearch/
[root@k8s-master elasticsearch]# tar -zxvf elasticsearch-6.4.2.tar.1.gz 
[root@k8s-node-1 elasticsearch]# cd elasticsearch-6.4.2//config
[root@k8s-master config]# vim elasticsearch.yml
```
```vim
cluster.name:ruigege           #集群名称
node.name:ruigege1           #节点名
network.host:192.168.1.9    #绑定的节点1地址
network.bind_host:0.0.0.0   #不设置这个，本机可能不能访问
discovery.zen.ping.unicast.hosts:["192.168.1.9","192.168.1.8"] #hosts列表
discovery.zen.minimum_master_nodes:1
#如下配置是为了解决Elasticsearch可视化工具dejavu的跨域问题，若不使用可视化工具那么就可忽略
http.port: 9200
http.cors.allow-origin:"http://192.168.199.76:1358"
http.cors.enabled:true
http.cors.allow-headers: X-Requested-With,X-Auth-Token,Content-Type,Content-Leng
th,Authorization
http.cors.allow-credentials:true
```
- 退出保存，然后设置一下节点2，只替换一下就行了
- 添加群组，设置权限，关闭防火墙，开启程序
```linux
[root@k8s-node-1 config]# groupadd es
[root@k8s-node-1 config]# useradd es -g es
[root@k8s-node-1 config]# cd ..
[root@k8s-node-1 elasticsearch-6.4.2]# cd ..
[root@k8s-node-1 elasticsearch]# chown -R es:es ./elasticsearch-6.4.2

[root@k8s-node-1 elasticsearch]# systemctl stop firewalld
[root@k8s-node-1 elasticsearch]# systemctl disable firewalld

[root@k8s-node-1 elasticsearch]# su es

[es@k8s-node-1 elasticsearch]$ cd elasticsearch-6.4.2/bin
[es@k8s-node-1 bin]$ ./elasticsearch
```
## 四、源码：
- 搭建一个开源项目12-Kubernetes集群部署(下)以及elasticsearch部署.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目12-Kubernetes集群部署(下)以及elasticsearch部署.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![100.0](https://img-blog.csdnimg.cn/20200808233919811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)

