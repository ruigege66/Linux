## 一、开始工具的安装
### 1.git
- 安装git工具有两种方式，一种就是利用自带包管理工具，一种是源码编译安装
- （1）由于CentOS已经具有包管理器因此只需要一行命令即可自动安装
```linux
yum install git
```
![3.1](https://imgkr2.cn-bj.ufileos.com/adbc3a21-d251-4f71-8475-7b9e7d506510.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=tfUO1V59aZEP%252BHUNBQctSYDokgE%253D&Expires=1596466714)
- (2)自行下载git安装包，进行安装
- 首先下载tar包，然后移动到root目录中
![3.2](https://imgkr2.cn-bj.ufileos.com/b4db1960-1e7b-4e67-abc6-a64bc9365e40.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=NWI953ci26RUvc8ozs%252Bhjwdo3yY%253D&Expires=1596468102)
- 从图中可见移动的轨迹，下面使用解压命令解压，得到目录git-2.28.0
```Linux
tar -zxvf 
```
> 复习tar是用来建立，还原备份文件的工具程序，它可以加入，解开备份文件内的文件。\
参数：\
-z或--gzip或--ungzip 通过gzip指令处理备份文件。\
-x或--extract或--get 从备份文件中还原文件\
-v或--verbose 显示指令执行过程。\
-f<备份文件>或--file=<备份文件> 指定备份文件。\
参考：[https://www.runoob.com/linux/linux-comm-tar.html](https://www.runoob.com/linux/linux-comm-tar.html)
- 接下来安装各种依赖
```Linux
yum install curl-devel gettext-devel openssl-devel zlib-devel gcc-c++ perl-ExtUtils-MakeMaker
```
![3.3](https://imgkr2.cn-bj.ufileos.com/bf5c4c8d-e898-474a-86f7-98ff5afc9c31.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=j9FhKWh96pPGttfcpDsX8lGOUB0%253D&Expires=1596468384)
- 把git工具进行编译安装，进入到目录git-2.28.0中，执行配置、编译、安装命令即可
```Linux
cd git-2.28.0
make configure
./configure --prefix=/usr/local/git
make profix=/usr/local/git
make install
```
> 复习：./configure 是用来检测你的安装平台的目标特征的。比如它会检测你是不是有CC或GCC，并不是需要CC或GCC，它是个shell脚本。 \
make 是用来编译的，它从Makefile中读取指令，然后编译。 \
make install是用来安装的，它也从Makefile中读取指令，安装到指定的位置。 \

- 接下来配置环境变量，git的可执行程序加入到环境变量
- 进入配置文件
```Linux
vim /etc/profile
```
- 在文件的尾部添加语句
```vim
export GIT_HOME=/user/local/git
export PATH=$PATH:$GIT_HOME/bin
```
- 最后执行`source /etc/profile`是环境变量生效
> 复习：1.在linux及unix的sh中，以$开头的字符串表示的是sh中定义的变量，这些变量可以是系统自动增加的，也可以是用户自己定义的$PATH表示的是系统的命令搜索路径，和windows的%path%是一样的$HOME则表示是用户的主目录，也就是用户登录后工作目录\
2.source 在当前bash环境下读取并执行FileName中的命令。\
*注：该命令通常用命令“.”来替代。\
使用范例：\
source filename \
. filename #（中间有空格）  
source命令（从 C Shell 而来）是bash shell的内置命令。点命令，就是个点符号，（从Bourne Shell而来）是source的另一名称。
同样的，当前脚本中配置的变量也将作为脚本的环境，source（或点）命令通常用于重新执行刚修改的初始化文档，如 .bash_profile 和 .profile 等等
引自：[https://www.cnblogs.com/xuange306/p/9436126.html](https://www.cnblogs.com/xuange306/p/9436126.html)
- 最后查看使用`git --version`查看安装结果
![3.4](https://imgkr2.cn-bj.ufileos.com/35ca24c8-77a9-46f9-8743-e19c9a4aa9d0.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=LVBjs4%252FjFXyC0N7X8lQTzJcIn1A%253D&Expires=1596470111)

## 二、源码：
- CSDN：[https://blog.csdn.net/weixin_44630050](https://blog.csdn.net/weixin_44630050)
- 博客园：[https://www.cnblogs.com/ruigege0000/](https://www.cnblogs.com/ruigege0000/)
- 欢迎关注微信公众号：傅里叶变换，个人账号，仅用于技术交流\
![127.59](https://static01.imgkr.com/temp/bd7c665638af480e97f18afd5062a416.jpg)
