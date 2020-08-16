## 一、规划
- 我们打算部署一个集群，**一主两从**的二节点Kubernetes集群，整体规划如下：

| 主机名 | IP地址 | 角色 |
| :------------ | :----------- | :------------ |
| k8s-master |  192.168.1.9 | k8s主节点 |
| k8s-node-1 |  192.168.1.8  | k8s从节点 |
- 所有节点都需要的环境：
	- (1)Docker版本:1.13.1;(2)Kubernetes版本：1.13.1;（3）kubelet(运行于所有的Node上，负责启动容器和Pod) （4）kubeadm(负责初始化集群)  （5）kubectl(k8s命令行工具，通过其可以部署/管理应用以及CRUD各种资源）
## 二、准备工作
- 所有节点关闭防火墙
```linux
[root@localhost ~]# systemctl disable firewalld.service
[root@localhost ~]# systemctl stop firewalld.service
```
- 禁用seLinux
```linux
[root@localhost ~]# setenforce 0
[root@localhost ~]# vi /etc/selinux/config
```
```vim
SELINUX=disabled
```
- 所有节点关闭swap
```linux
[root@localhost ~]# swapoff -a
```
- 设置所有节点主机名
```linux
[root@localhost ~]# hostnamectl --static set-hostname k8s-master
[root@localhost ~]# hostnamectl --static set-hostname k8s-node-1
```
- 所有节点 主机名/IP加入hosts解析
```linux
[root@localhost ~]# vim /etc/hosts
```
```vim
192.168.1.9 k8s-master
192.168.1.8 k8s-node-1
```
## 三、组件安装
- docker安装，之前的连载已经OK了，这里不再赘述
### 1.安装kubelet、kubeadm、kubectl
- 首先准备repo
```linux
[root@localhost ~]# cat>>/etc/yum.repos.d/kubrenetes.repo<<EOF
> [kubernetes]
> name=Kubernetes Repo
> baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
> gpgcheck=0
> gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
> EOF
```
- 然后执行如下命令来进行安装
```linux
[root@localhost ~]# setenforce 0
[root@localhost ~]# sed -i 's/^SELINUX=enforcing$/SELINUX= disabled/' /etc/selinux/config
[root@localhost ~]# yum install -y kubelet kubeadm kubectl
```
- 未完待续
## 四、源码：
- 搭建一个开源项目10-Kubernetes集群部署（上）.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目10-Kubernetes集群部署（上）.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![100.0](https://img-blog.csdnimg.cn/20200808233919811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)
