## 一、打造一个多节点的Linux环境
### 1.需要的软件以及环境
- [阿里云开发者社区](https://developer.aliyun.com/mirror/)
- VMware Fusion（虚拟机）\
[下载地址：http://mirrors.aliyun.com/centos/7/isos/x86_64/](http://mirrors.aliyun.com/centos/7/isos/x86_64/)
- CentOS（Linux镜像）\
[下载地址：https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html](https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html)
- SecureCRT（SSH中端工具）
- Transmit(传输文件)
### 2.开始安装
- 首先先下载好我们的工具
![1.0](https://imgkr.cn-bj.ufileos.com/11598488-5317-464e-a663-474a485ec703.png)
- 然后安装好VMWare即可开始使用虚拟机了
- 添加CentOS镜像
![1.09](https://imgkr.cn-bj.ufileos.com/cfd8c6e3-a972-4ea5-94e0-9dc91c72c8f2.png)

![1.1](https://imgkr.cn-bj.ufileos.com/4fd38227-291b-440a-a534-4d6c0b489a24.png)
- 然后设置我们的内存（2GB），核心数量（双核），硬盘大小（50GB），这个没有固定标准，只要够用就行哦
![1.2](https://imgkr.cn-bj.ufileos.com/d7423ce9-3b2d-47c5-8842-8eac570ec56d.png)
![1.3](https://imgkr.cn-bj.ufileos.com/6818f55a-cef8-41d8-9778-77a863de623f.png)
- 都设置好了之后开始初始化
![1.4](https://imgkr.cn-bj.ufileos.com/d2d2029b-4662-43eb-95f5-fec17afe2248.png)
- 选择语言，安装必备的软件（这个依自己而定，也可以参考我的）
![1.5](https://imgkr.cn-bj.ufileos.com/b9c43e6d-89ee-4abb-9e87-ddc7c1ecb416.png)
![1.6](https://imgkr.cn-bj.ufileos.com/14e8d30c-47fc-42b0-a8ef-830703645ec6.png)
![1.7](https://imgkr.cn-bj.ufileos.com/addd5e55-7b86-4b57-b981-64bc367a186c.png)
![1.8](https://imgkr.cn-bj.ufileos.com/3502d046-1cce-4ea9-a1a4-f8a3be40a44b.png)
- 最后我这里的分区的方式选择默认的方式，开始安装
![1.9](https://imgkr.cn-bj.ufileos.com/c67b24a5-9c35-45af-85e5-98b30dad4a2e.png)
- 安装好之后，选择可以联网
![1.10](https://imgkr.cn-bj.ufileos.com/7900a4da-d987-466b-a8ee-621c96bce780.png)
- 安装好之后的界面就是这样的
![1.11](https://imgkr.cn-bj.ufileos.com/1ee58034-8d0e-4ea3-831e-f12c711cabfa.png)
### 2.开始运行
- 调用终端窗口，输入ifconfig,查看宿主机的IP地址
![1.12](https://imgkr.cn-bj.ufileos.com/36a7a842-1b38-4dbd-a05d-3b4e21870178.png)
- 也可以使用快捷键ctrl+alt+F2,或者F1，用于切换终端和用户界面。
- 我们要给虚拟机一个具体的IP地址，不能让它每次都变化，我们使用工具来随机分配生成一个
- 首先切换到root模式,命令行：su root
- 使用工具dhclient来分配,命令函：dhclient
![1.13](https://imgkr.cn-bj.ufileos.com/7d5613fe-704f-454c-9b49-b330233b3f42.png)
- 查看IP地址为：192.168.159.128
- 编辑虚拟的网卡，使这个地址变为静态地址，命令行:vim /etc/sysconfig/network-scripts/ifcfg
- 然后选择一个网卡进去，修改一个属性BOOTPROTO为static，让它的IP地址变为静态的，这样地址就不会随机改变了。
![1.14](https://imgkr.cn-bj.ufileos.com/91ec4233-770a-45b2-9a86-f7ed0ab40079.png)
- 接下来，修改ONBOOT为yes
- 添加IPADDR=192.168.159.128
- NETMASK=255.255.255.0
- GATEWAY=192.168.159.1
- DNS1=119.29.29.29
- 命令行wq退出
- 然后重启网卡，命令行：systemctl restart network.service

## 二、源码：
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![127.59](https://static01.imgkr.com/temp/bd7c665638af480e97f18afd5062a416.jpg)
