## 配置Oracle Yum源
```sh
cd /etc/yum.repos.d
sudo wget http://yum.oracle.com/public-yum-ol7.repo
sudo wget http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol7 -O /etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
```
## 执行Oracle预安装程序
```sh
sudo yum install -y oracle-rdbms-server-11gR2-preinstall
```
## 配置Oracle操作系统用户
以下操作以root用户登录
1. 以root用户登录操作系统
2. 执行以下命令修改oracle用户密码，需要连续输入两次oracle的新密码
```
passwd oracle
```
3. 修改文件/etc/selinux/config，确认参数`SELINUX`的值为`permissive`
```
SELINUX=permissive
```
4. 执行以下命令
```
setenforce Permissive
```
5. 禁用防火墙
```
systemctl stop firewalld
systemctl disable firewalld
```
6. 执行以下命令创建Oracle安装目录
```
mkdir -p /u01/app/oracle/product/11.2.0.4/db_1
chown -R oracle:oinstall /u01
chmod -R 775 /u01
```
6. 修改文件/home/oracle/.bash_profile，添加以下内容
```
TMP=/tmp; export TMP
TMPDIR=$TMP; export TMPDIR

ORACLE_UNQNAME=WIND; export ORACLE_UNQNAME
ORACLE_BASE=/u01/app/oracle; export ORACLE_BASE
ORACLE_HOME=$ORACLE_BASE/product/11.2.0.4/db_1; export ORACLE_HOME
ORACLE_SID=WIND; export ORACLE_SID
ORACLE_TERM=xterm; export ORACLE_TERM
PATH=/usr/sbin:$PATH; export PATH
PATH=$ORACLE_HOME/bin:$PATH; export PATH

LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib; export LD_LIBRARY_PATH
CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib; export CLASSPATH
```
## 安装Oracle数据库软件
1. 以oracle用户登录操作系统
2. 将Oracle 11.2.0.4安装文件拷贝到服务器
3. 执行命令解压Oracle安装文件
```
unzip p13390677_112040_Linux-x86-64_1of7.zip
unzip p13390677_112040_Linux-x86-64_2of7.zip
```
3. 打开终端，进入到oracle安装文件目录
4. 执行以下命令启动安装程序
```
./runInstaller
```

## 创建Oracle数据库实例

