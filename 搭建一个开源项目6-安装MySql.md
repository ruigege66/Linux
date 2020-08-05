## 一、安装MySql
- 将安装包`mysql-5.7.30-linux-glibc2.12-x86_64.tar.gz`放在`/root`下
- 首先卸载自带的`Mariadb`
```Linux
[root@localhost ~]# rpm -qa | grep mariadb
```
![6.1](https://imgkr2.cn-bj.ufileos.com/78435cd8-7e6a-4665-9aef-d92d25c93a55.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=%252BYNQk3JUmRk2NWpor1Db5N4iE3g%253D&Expires=1596725995)
```Linux
[root@localhost ~]# yum -y remove mariadb-server-5.5.65-1.el7.x86_64
[root@localhost ~]# yum -y remove mariadb-5.5.65-1.el7.x86_64
[root@localhost ~]# yum -y remove mariadb-libs-5.5.65-1.el7.x86_64
[root@localhost ~]# yum -y remove mariadb-devel-5.5.65-1.el7.x86_64
```
## 二、解压MySql安装包
- 安装包解压到`/usr/local/`,并重命名mysql
```Linux
[root@localhost ~]# tar -zxvf /root/mysql-5.7.30-linux-glibc2.12-x86_64.tar.gz -C /usr/local/
[root@localhost local]# mv mysql-5.7.30-linux-glibc2.12-x86_64 mysql
```
- 创建MySql用户和用户组
```Linux
[root@localhost mysql]# groupadd mysql
[root@localhost mysql]# useradd -g mysql mysql
```
- 新建`/usr/local/mysql/data`目录，后续备用
```Linux
[root@localhost mysql]# mkdir data
```
- 修改MySql目录的归属用户
```Linux
[root@localhost mysql]# chown -R mysql:mysql ./
```
- 修改配置文件，在`/etc`目录下新建`my.cnf`文件
```vim
[mysql]
#设置mysql客户端默认字符集
default-character-set=utf8
socket=/var/lib/mysql/mysql.sock

[mysqld]
skip-name-resolve
#设置3306端口
port=3306
socket=/var/lib/mysql/mysql.sock
#设置mysql的安装目录
basedir=/usr/local/mysql
#设置mysql数据库的数据的存放目录
datadir=/usr/local/mysql/datadir
#允许最大连接数
max_connections=200
#服务端使用的字符集默认为8比特编码的latinl字符集
character-set-server=utf8
#创建新表时将使用默认的存储引擎
default-storage-engine=INNODB
lower_case_table_names=1
max_allowed_packet=16M
```
- 创建`/var/lib/mysql`目录，并修改权限
```Linux
[root@localhost etc]# mkdir /var/lib/mysql
[root@localhost etc]# chmod 777 /var/lib/mysql
```
- 开始正式安装mysql
[root@localhost etc]# cd /usr/local/mysql
[root@localhost mysql]# ./bin/mysqld --initiailize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
```
- 复制启动脚本到资源目录
```Linux
[root@localhost mysql]# cp ./support-files/mysql.server /etc/init.d/mysqld
```
- 修改`/etc/init.d/mysqld`，修改其`basedir`和`datadir`为实际对应目录
```vim
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
```
## 三、设置MySql系统服务并开启自启动
- 首先增加`mysqld`服务控制脚本执行权限,并将`mysql`服务加到系统服务
```Linux
[root@localhost mysql]# chmod +x /etc/init.d/mysqld
[root@localhost mysql]# chkconfig --add mysqld
```
- 最后检查`mysql`服务是否已经生效即可
```Linux
[root@localhost mysql]# chkconfig --list mysqld
```
![6.2](https://imgkr2.cn-bj.ufileos.com/a74766b1-18e9-481a-b0bf-b31f9eaea424.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=ZB5dlzOCq3%252BZh2MCQcZIEuGQdtM%253D&Expires=1596729736)
- 上面这个图说明`mysqld`服务已经生效了，在2，3，4，5运行级别随系统启动而自动启动，以后可以直接使用`service`命令控制mysql的启动停止
- 下次接着演示mysql的启动关闭

## 四、源码：
- 搭建一个开源项目6-安装MySql.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目6-安装MySql.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![127.59](https://static01.imgkr.com/temp/bd7c665638af480e97f18afd5062a416.jpg)
