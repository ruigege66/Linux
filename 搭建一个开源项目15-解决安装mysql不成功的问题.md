## 一、解决mysql安装不成功的问题
- 今天晚上就干了一件事，之前安装mysql不成功的问题
- 原来是`/usr/local/mysql/data`目录下面有东西，删除了就好了
- 然后执行下面的语句，生成临时密码`E<OspCjN-0of`

```linux
[root@k8s-master data]# cd /usr/local/mysql/bin
[root@k8s-master bin]# ./mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql
[root@k8s-master bin]# service mysqld start
```
![15.1](https://img-blog.csdnimg.cn/20200902235847212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70#pic_center)
- 接着将mysql的bin文件夹，添加到环境变量中,并且重启该文件
```linux
[root@k8s-master bin]# vim ~/.bash_profile 
[root@k8s-master bin]# source ~/.bash_profile 

```
```vim
#mysql
export PATH=$PATH:/usr/local/mysql/bin
```
- 首次登录mysql,以root账户登录，并使用上文中出现的临时密码
```linux
[root@k8s-master bin]# mysql -u root -p
```
![15.2](https://img-blog.csdnimg.cn/2020090300045338.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70#pic_center)
- 接下来修改密码
```mysql
mysql> alter user user() identified by "密码";
mysql>  flush privileges;
```
![15.3](https://img-blog.csdnimg.cn/20200903000814603.png#pic_center)
```mysql
mysql> use mysql;
mysql> update user set user.Host='%' where user.User='root';
mysql> flush privileges;
```
- 使用navicat测试是否可以调通
![15.4](https://img-blog.csdnimg.cn/20200903001238742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70#pic_center)
## 二、源码：
- 搭建一个开源项目15-解决安装mysql不成功的问题.md
- https://github.com/ruigege66/Linux/blob/master/搭建一个开源项目15-解决安装mysql不成功的问题.md
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![100.0](https://img-blog.csdnimg.cn/20200808233919811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYzMDA1MA==,size_16,color_FFFFFF,t_70)


