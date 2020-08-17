## 一、MASTER节点配置
### 1.初始化k8s集群
- 为了应对网络不畅通的问题，我们国内网络环境只能提前手动下载相关镜像并重新打tag
```linux
[root@k8s-master ~]# docker pull mirrorgooglecontainers/kube-apiserver:v1.13.1
[root@k8s-master ~]# docker pull mirrorgooglecontainers/kube-controller-manager:v1.13.1
[root@k8s-master ~]# docker pull mirrorgooglecontainers/kube-scheduler:v1.13.1
[root@k8s-master ~]# docker pull mirrorgooglecontainers/kube-proxy:v1.13.1
[root@k8s-master ~]# docker pull mirrorgooglecontainers/pause:3.1
[root@k8s-master ~]# docker pull mirrorgooglecontainers/etcd:3.2.24
[root@k8s-master ~]# docker pull coredns/coredns:1.2.6
[root@k8s-master ~]# docker pull registry.cn-shenzhen.aliyuncs.com/cp_m/flannel:v0.10.0-amd64

[root@k8s-master ~]# docker tag mirrorgooglecontainers/kube-apiserver:v1.13.1 k8s.gcr.io/kube-apiserver:v1.13.1
[root@k8s-master ~]# docker tag mirrorgooglecontainers/kube-controller-manager:v1.13.1 k8s.gcr.io/kube-controller-manager:v1.13.1
[root@k8s-master ~]# docker tag mirrorgooglecontainers/kube-scheduler:v1.13.1 k8s.gcr.io/kube-scheduler:v1.13.1
[root@k8s-master ~]# docker tag mirrorgooglecontainers/kube-proxy:v1.13.1 k8s.gcr.io/kube-proxy:v1.13.1
[root@k8s-master ~]# docker tag mirrorgooglecontainers/pause:3.1 k8s.gcr.io/pause:3.1
[root@k8s-master ~]# docker tag mirrorgooglecontainers/etcd:3.2.24 k8s.gcr.io/etcd:3.2.24
[root@k8s-master ~]# docker tag coredns/coredns:1.2.6 k8s.gcr.io/coredns:1.2.6
[root@k8s-master ~]# docker tag registry.cn-shenzhen.aliyuncs.com/cp_m/flannel:v0.10.0-amd64 quay.io/coreos/flannel:v0.10.0-amd64

[root@k8s-master ~]# docker rmi mirrorgooglecontainers/kube-apiserver:v1.13.1
[root@k8s-master ~]# docker rmi mirrorgooglecontainers/kube-controller-manager:v1.13.1
[root@k8s-master ~]# docker rmi mirrorgooglecontainers/kube-scheduler:v1.13.1
[root@k8s-master ~]# docker rmi mirrorgooglecontainers/kube-proxy:v1.13.1
[root@k8s-master ~]# docker rmi mirrorgooglecontainers/pause:3.1
[root@k8s-master ~]# docker rmi mirrorgooglecontainers/etcd:3.2.24
[root@k8s-master ~]# docker rmi coredns/coredns:1.2.6
[root@k8s-master ~]# docker rmi registry.cn-shenzhen.aliyuncs.com/cp_m/flannel:v0.10.0-amd64
```
- 然后再在Master节点上执行如下命令初始化k8s集群：
```linux
[root@k8s-master ~]# kubeadm init --kubernetes-version=v1.13.1 --apiserver-advertise-address 192.168.1.9 --pod-network-cidr=10.244.0.0/16
```
- kubernets-version:用于指定k8s版本
- apiserver-advertise-address:用于指定使用master的哪个network interface进行通信，如果不指定的话，则kubeadm会自动选择具有默认网关的interface
- pod-network-cidr:用于指定pod的网络范围，该参数使用依赖于使用的网络方案，本文将会使用经典的flannel网络方案
![11.1](https://img-blog.csdnimg.cn/202008180020573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70#pic_center)

## 二、源码：
- 搭建一个开源项目10-Kubernetes集群部署（中）.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目10-Kubernetes集群部署（中）.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![100.0](https://img-blog.csdnimg.cn/20200808233919811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
