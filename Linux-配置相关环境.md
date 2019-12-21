### 安装Oracle客户端驱动程序
[官网链接](https://www.oracle.com/technetwork/cn/topics/linuxx86-64soft-095635-zhs.html)
#### 官网配置教程
1. 下载所需的 Instant Client ZIP 文件。所有安装都要求 Basic 或 Basic Light 软件包。
```
wget http://xx
```
2. 将软件包解压缩到应用可以访问的一个目录中，如 /opt/oracle/instantclient_12_2。例如：
```
cd /opt/oracle			
unzip instantclient-basic-linux.x64-12.2.0.1.0.zip
```
3. 如果不存在到 Instant Client 版本的相应链接，请创建链接。例如：
```
  cd /opt/oracle/instantclient_12_2
  ln -s libclntsh.so.12.1 libclntsh.so
  ln -s libocci.so.12.1 libocci.so
```
4. 安装 libaio 软件包。这在某些 Linux 版本中名为 libaio1。
```
例如，在 Oracle Linux 上，运行：

  sudo yum install libaio
```
5. 如果 Instant Client 是此系统上安装的唯一 Oracle 软件，则更新运行时链接路径，例如：
```

  sudo sh -c "echo /opt/oracle/instantclient_12_2 > \
      /etc/ld.so.conf.d/oracle-instantclient.conf"
  sudo ldconfig
  
  或者，在运行应用之前设置 LD_LIBRARY_PATH 环境变量。例如：
  
    export LD_LIBRARY_PATH=/opt/oracle/instantclient_12_2:$LD_LIBRARY_PATH
  
  可以选择将该变量添加到配置文件（如 ~/.bash_profile）和应用配置文件（如 /etc/sysconfig/httpd）。
```

6. 如果您打算将可选的 Oracle 配置文件（如 tnsnames.ora、sqlnet.ora ldap.ora 或 oraaccess.xml）与 Instant Client 放在同一位置，那么请创建一个 network/admin 子目录。例如：

```
  mkdir -p /opt/oracle/instantclient_12_2/network/admin
  
  这是与此 Instant Client 链接的应用的默认 Oracle 配置目录。
  
  或者，Oracle 配置文件可以放在另一个可访问的目录中。然后，将环境变量 TNS_ADMIN 设置为该目录名称。
```

7. 要使用 SQL*Plus 软件包中的二进制文件（如 sqlplus），请将软件包解压缩到 Basic 软件包所在的目录，然后更新您的 PATH 环境变量，例如：
```

  export PATH=/opt/oracle/instantclient_12_2:$PATH
```

8. 启动应用。

#### 实际提炼的配置教程(推荐使用rpm安装)
1. 下载所需的 Instant Client ZIP 文件。所有安装都要求 Basic 或 Basic Light 软件包。
2. 将软件包解压缩到应用可以访问的一个目录中，如 /opt/oracle/instantclient_12_2。例如：
```

cd /opt/oracle			
unzip instantclient-basic-linux.x64-12.2.0.1.0.zip---注意解压到指定的目录下
```
3. 编辑./bash_profile文件(或者创建环境变量文件/etc/profile.d/oracle.sh)
	+ vim /root/.bash_profile
	```
	export ORACLE_HOME=/usr/lib/oracle/11.2/client64
	export TNS_ADMIN=$ORACLE_HOME/lib/network/admin
	export PATH=$PATH:$ORACLE_HOME/bin
	export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$ORACLE_HOME/lib
	```
	+ source  /root/.bash_profile使配置生效
4. 创建快捷链接。例如：
```
  cd $ORACLE_HOME
  ln -s libclntsh.so.11.1 libclntsh.so
  ln -s libocci.so.11.1 libocci.so

```
5. 创建tns配置文件/opt/oracle/instantclient_11_2/tnsnames.ora,增加如下内容:
```
mark=
(DESCRIPTION=
(ADDRESS=(PROTOCOL=TCP)(HOST=103.5.193.214)(PORT=21021))
(CONNECT_DATA=
(SERVER=DEDICATED)
(SERVICE_NAME=ORCL)
)
)
```
6. 创建目录
```
mkdir -p /usr/lib/oracle/12.2/client64/lib/network/admin
```
7. 查看并杀死进程
```
ps -ef | grep firefox
pgrep firefox
kill -s 9 1827
```
### 安装Python3和Python2
0. 安装依赖包
```
gcc --version  查看------没安装的先安装gcc，yum -y install gcc
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel
```
1. 官网下载Python3源码--[链接](https://www.python.org/ftp/python/3.7.5/)
```
wget https://www.python.org/ftp/python/3.7.5/Python-3.7.5.tgz
```
2. 解压
```
tar -zxvf Python-3.7.0.tgz
```
3. 建立一个空文件夹，用于存放python3程序　(这里可以不用事先创建空文件夹，第4步会自动创建)
```
mkdir /usr/local/python3 
```
4. 执行配置文件，编译，编译安装　
```
cd Python-3.7.0
./configure --prefix=/usr/local/python3
make && make install
```
5. 建立软连接(有的Python2.x不自带pip，需要手动安装)
```
ln -s /usr/local/python3/bin/python3.7 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3.7 /usr/bin/pip3　　
```
6. 为Python2.X单独安装pip再进行上一步的配置
```
[官网链接](https://pypi.org/project/pip/)
### 先下载setuptools源码包
### 再下载pip源码包
### 解压源码包
tar -zxvf pip-19.3.1.tar.gz
unzip setuptools-42.0.2.zip----如果没有这个命令，使用 yum -y install unzip
cd setuptools-42.0.2
python27 setup.py install
cd pip-19.3.1
python27 setup.py install
ln -s /usr/local/python27/bin/pip2.7 /usr/bin/pip27
```
### 配置python2和python3，并获得共存
1. ln -s python3的路径 /usr/bin/python
	+ 如果提示: failed to create symbolic link '/usr/bin/python': File exists 
	+ 先删除: rm -rf /usr/bin/python
	+ 再操作一次: ln -s python3的路径 /usr/bin/python
2. 查看当前配置
	+ ll python  显示当前python的指向
	+ python -V 与 python2 -V  分别打印出两个版本的相关信息
3. 配置pip共存
	+ ln -s pip3的路径 /usr/bin/pip3   
		1. 如: ln -s /usr/local/python3/bin/pip3.6 /usr/bin/pip3   
	+ 此时pip2对应python2，pip3对应python3
	+ 如果不配置pip共存
		1. 使用pip install xxx  默认是会将库安装到python2中的
		2. python -m install XXXX  这样才是安装到python3中


### Centos Linux系统 安装 python3 (Anacada)
1. 下载 Anaconda/Minconda 安装包 (建议使用国内镜像)
```
# Anaconda & Minconda 二选一
# Anaconda
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-5.0.1-Linux-x86_64.sh
 
# Minconda
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
2. 安装
```
bash Miniconda3-latest-Linux-x86_64.sh

### 一直 Enter、yes 安装即可，然后下面刷新一下配置环境。
source  /root/.bash_profile

### 报错
1. 如果报 bzip 的错，安装后重试上述安装步骤。（自己安装的时候出现了这个问题）  
	+ 解决方案  sudo yum install -y bzip2
```
3. 检验是否安装成功
```
conda list
```

### Linux设置定时任务
1. 编辑linux中的定时任务
	+ 可用crontab -e命令打开编辑任务栏来编辑---编辑的是/var/spool/cron下对应用户的cron文件
	+ 也可以直接修改/etc/crontab文件
2. 查看任务是否启动命令：service crond status
3. crond没有启动成功，需要使用命令： service crond start 来启动crond任务
4. 当使用命令：service crond start 后 crond任务任然处于未启动状态：
	+ pkill cron 来强杀干扰crond任务启动的所有进程
	+ 然后再执行命令：service crond start 
5. cron的相关命令
	+ service crond start    //启动服务
	+ service crond stop     //关闭服务
	+ service crond restart  //重启服务
	+ service crond reload   //重新载入配置
	+ service crond status   //查看服务状态 

6. 解决Linux定时任务不能正常运行
	+ 检查服务状态开启服务
		1. service crond status
	+ 命令使用绝对路径
		1. 要么将命令的绝对路径添加到PATH中
		2. 要么就直接使用绝对路径的命令
	+ 添加根目录
		1. 命令操作的脚本尽量使用绝对路径
	+ 时间设定 分 时 需要统一，单数都用单数，双数都用双数  版本问题，有的Linux系统不识别
		1. 0 9 * * * 
		2. 35 10 * * * 
7. 查看Python进程是否在运行
```
ps -ef | grep python3
```
8. 神坑报错
```
### DPI-1047: Cannot locate a 64-bit Oracle Client library: "libclntsh.so: cannot open shared object file

### 问题解析
	+ 这个报错的意思是  无法定位到Oracle的lib库
	
### 解决(还是看官方文档有用)
For Instant Client 19 RPMs, the system library search path is automatically configured during installation.

For older versions, if there is no other Oracle software on the machine that will be impacted, permanently add Instant Client to the runtime link path. For example, with sudo or as the root user:

sudo sh -c "echo /usr/lib/oracle/18.5/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf"
sudo ldconfig
Alternatively, for version 18 and earlier, every shell will need to have the environment variable LD_LIBRARY_PATH set to the appropriate directory for the Instant Client version. For example:

export LD_LIBRARY_PATH=/usr/lib/oracle/18.5/client64/lib:$LD_LIBRARY_PATH

	+ version 18 and earlier，两种配置方式
		1. export LD_LIBRARY_PATH=/usr/lib/oracle/18.5/client64/lib:$LD_LIBRARY_PATH
			* 这种可能需要配置后重启计算机，我由于没有重启所以配置了也没有生效(source  /root/.bash_profile 也尝试过了)
			* 或者可以创建环境变量文件/etc/profile.d/oracle.sh进行尝试
		2.  sudo sh -c "echo /usr/lib/oracle/18.5/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf"(推荐)
			sudo ldconfig
	+ Instant Client 19 RPMs  自动会配置lib链接路径
```