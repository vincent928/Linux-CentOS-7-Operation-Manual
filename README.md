# Linux __CentOS 7 Operation Manual
 ![](https://github.com/vincent928/Linux-CentOS-7-Operation-Manual/blob/master/pic/linux.jpg)


* [Linux __CentOS 7 Operation Manual](#linux-__centos-7-operation-manual)
  * [一.基本指令](#一基本指令)
    * [1.进程服务相关操作](#1进程服务相关操作)
* [查看监听端口号与对应服务名称](#查看监听端口号与对应服务名称)
    * [2.file相关操作](#2file相关操作)
    * [3.vim相关操作](#3vim相关操作)
    * [4.下载相关操作](#4下载相关操作)
  * [二.MySQL5.7 安装](#二mysql57-安装)
  * [三.FTP 安装](#三ftp-安装)
    * [1.yum安装vsftpd](#1yum安装vsftpd)
    * [2.配置文件](#2配置文件)
    * [3.服务开机自启](#3服务开机自启)
    * [4.服务端口号查看](#4服务端口号查看)
    * [5.配置vsftpd](#5配置vsftpd)
    * [6.安全组设置](#6安全组设置)
    * [7.服务安全加固](#7服务安全加固)
      * [1).禁用匿名登录服务](#1禁用匿名登录服务)
      * [2).禁止显示Banner信息](#2禁止显示banner信息)
      * [3).限制FTP登录用户](#3限制ftp登录用户)
      * [4).限制FTP用户目录](#4限制ftp用户目录)
      * [5).修改监听地址和默认端口](#5修改监听地址和默认端口)
      * [6).启用日志记录](#6启用日志记录)
      * [7).其他安全设置](#7其他安全设置)
  * [四.Redis 安装](#四redis-安装)
  * [五.RabbitMQ 安装](#五rabbitmq-安装)
  * [六.Nginx 安装](#六nginx-安装)
    * [1.安装](#1安装)
      * [1)环境安装](#1环境安装)
      * [2)源码安装](#2源码安装)
      * [3)Https](#3https)
  * [七.GitLab 安装](#七gitlab-安装)
  * [八.SVN 安装](#八svn-安装)
  * [九.Git+Node.js安装](#九gitnodejs安装)
  * [十.用户权限控制](#十用户权限控制)


----

## 一.基本指令
### 1.进程服务相关操作

**systemctl就是service和chkconfig这两个命令的整合**

任务|旧指令|新指令
-|-|-
A服务自启|chkconfig --level 3 A on|systemctl enable A.service
A服务不自启|chkconfig --level 3 A off|systemctl disable A.service
显示所有已启动的服务|chkconfig --list|systemctl list-units --type=service
A服务启动|service A start|systemctl start A.service
A服务状态|service A status|systemctl status A.service
A服务停止|service A stop|systemctl stop A.service
A服务重启|service A restart|systemctl restart A.service

**netstat**
```shell
# 查看监听端口号与对应服务名称
netstat -antp
```
### 2.file相关操作
![](https://github.com/vincent928/Linux-CentOS-7-Operation-Manual/blob/master/pic/filesystem.png)
(https://www.cnblogs.com/123-/p/4189072.html)

**tar** : **文件压缩解压缩相关**
```shell
*.tar          用 tar -xvf 解压
*.tar.gz/*.tgz 用 tar -zxzf 解压
*.tar.bz2      用 tar -xjf 解压
*.tar.Z        用 tar -xZf 解压
*.gz           用 gzip -d 或者 gunzip 解压
*.bz2          用 bzip2 -d 或者 bunzip2 解压
*.Z            用 uncompress 解压
*.rar          用 unrar e解压
*.zip          用 unzip 解压
```

### 3.vim相关操作
(https://blog.csdn.net/hongwei15732623364/article/details/80591140)
### 4.下载相关操作

**wget** : **主要用于下载**
```shell
wget [a] [c] url
[a] : [c] : 说明
  -c : - : 继续下载部分下载的文件
  -O ：/var/xxx.zip : 下载,并保存文件至指定文件夹与文件名 
  -b : - : 后台下载,可以使用 tail -f wget-log 查看进度
  -S : - : 打印服务器响应,模拟下载,非真实下载,用于检查请求地址的网络状态 
```
### 5.用户相关操作

**useradd** : **用于用户建立**
```shell
useradd [-mMnr][-c <备注>][-d <登入目录>][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-s <shell>][-u <uid>][用户帐号]
```
**参数说明:**
```shell
-c <备注> 　   加上备注文字。备注文字会保存在passwd的备注栏位中。
-d <登入目录>  指定用户登入时的启始目录。
-D 　          变更预设值．
-e <有效期限>  指定帐号的有效期限。
-f <缓冲天数>  指定在密码过期后多少天即关闭该帐号。
-g <群组>      指定用户所属的群组。
-G <群组>      指定用户所属的附加群组。
-m 　          自动建立用户的登入目录。
-M 　          不要自动建立用户的登入目录。
-n 　          取消建立以用户名称为名的群组．
-r 　          建立系统帐号。
-s <shell>　 　指定用户登入后所使用的shell。
-u <uid> 　    指定用户ID。
[用户账号]      用户名,例如mysqltest,rabbitmqtest,svntest等
```

## 二.MySQL5.7.24 安装
### 0.卸载系统自带的数据库
```shell
rpm -qa | grep mariadb
#mariadb-libs-5.5.52-1.el7.x86_64
#卸载
yum -y remove mariadb*
```

### 1.mysql5.7.24安装
前往[官网](https://dev.mysql.com/downloads/mysql/5.7.html#downloads)下载5.7.24版本,CentOS则选择操作系统为`Linux-Generic`(Linux通用版本),64位还是32位自行选择。选择Compressed TAR Archive下载。或者使用wget直接下载
```shell
wget https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz
```
将压缩包移动到`/usr/local`下,解压缩,并改名为mysql
```shell
mv mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz /usr/local
cd /usr/local
#解压缩
tar -zxvf mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz
#删除压缩包
rm -f mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz
#重命名
mv mysql-5.7.24-linux-glibc2.12-x86_64 /mysql
```

### 2.mysql用户添加
添加mysql用户及用户组
```shell
#先检查是否有mysql用户及用户组
groups mysql
groupadd mysql
useradd -r -g mysql mysql
groups mysql
#mysql : mysql
#修改/usr/local/mysql所属用户
chown -R mysql.mysql /usr/local/mysql/
```

### 3.配置mysql服务
[理解Linux系统/etc/init.d目录和/etc/rc.local脚本](https://blog.csdn.net/acs713/article/details/7322082)
```shell
#添加到服务
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
vim /etc/init.d/mysqld
```
**添加mysql与data路径**
```shell
#大概在46行位置
basedir = /usr/local/mysql
datadir = /usr/local/mysql/data
```
```shell
#设置开机自启
chkconfig --add mysqld
chkconfig mysqld on 
```    
### 4.配置mysql配置文件

```shell
cd /etc
#创建配置文件
vim my.cnf
```
**配置内容**
```shell
[client]
port = 3306
default-character-set = utf8
socket = /tmp/mysql.sock

[mysqld]
basedir = /usr/local/mysql
datadir = /usr/local/mysql/data
port = 3306
character-set-server = utf8
default_storage_engine = InnoDB
server-id = 1
```

### 5.初始化数据库
```shell
cd /usr/local/mysql
mkdir tmp
cd bin
#初始化
./mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
#如果报错请根据报错安装对应依赖包
#error while loading shared libraries: libnuma.so.1
#yum -y install numactl.x86_64
#error while loading shared libraries: libaio.so.1
#yum -y install libaio
#重新初始化
```
**执行完毕以后,信息最后一行会有初始密码**
```shell
# WXt2dhv#yi1c
2018-12-27T08:35:15.056910Z 1 [Note] A temporary password is generated for root@localhost: WXt2dhv#yi1c
```
### 6.启动数据库服务
```shell
#启动mysql
systemctl start mysqld
systemctl status mysqld
```

### 7.登录及远程登录配置
```shell
#进入/usr/local/mysql/bin 下执行
cd /usr/local/mysql/bin
#连接服务
./mysql -uroot -p
#输入初始密码
#mysql>
#修改密码
set password=password('新密码');
#退出
quit;
#重新输入密码,看看是否生效
#./mysql -uroot -p
```
```shell
#开启远程登录
#mysql>
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '访问密码';
#配置生效
flush privileges;
```
### 8.安全组
控制台查看安全组是否放行了mysql的端口3306(可以更改)。用client连接测试通过则完成。

## 三.FTP 安装
### 1.yum安装vsftpd
```shell
yum install -y vsftpd
```
### 2.配置文件
yum安装的ftp配置文件位置为`/etc/vsftpd`，有如下文件：
```shell
/etc/vsftpd/ftpusers      #黑名单配置文件,不允许访问ftp服务器的用户列表
/etc/vsftpd/user_list     #控制访问ftp服务器的用户列表
/etc/vsftpd/vsftpd.conf   #核心配置文件
/etc/vsftpd/vsftpd_conf_migrate.sh
```
### 3.服务开机自启
设置服务开机自启
```shell
systemctl enable vsftpd.service
```
### 4.服务端口号查看
```shell
netstat -antup|grep vsftpd
```
### 5.配置vsftpd
vsftpd安装完成以后，默认配置为：
1. 允许匿名用户和本地用户登录。
2. 匿名用户的登录名为ftp或anonymous，口令为空;匿名用户不能离开用户home目录`/var/ftp`,并且只能下载而无法上传。
3. 本地用户登录名为本地用户名，口令为对应用户口令。本地用户可以在自身的home目录中读写;本地用户可以离开自身home目录并切换到有权限的访问的其他目录。并在权限允许的情况下进行读写操作。
4. `/etc/vsftpd/ftpusers`文件中的本地用户禁止登陆

* **配置匿名用户上传文件权限**

1.修改`/etc/vsftpd/vsftpd.conf`的配置文件,赋予匿名FTP更多功能
```shell
#是否允许写权限
write_enable=YES
#是否允许匿名用户上传文件
anon_upload_enable=YES
```
2.更改目录权限,为ftp用户添加写权限,并重启服务
```shell
chmod o+w /var/ftp/pub
systemctl restart vsftpd.service
```
     
* **配置本地用户登录**

本地用户登录就是指用户使用Linux操作系统中的用户账号和密码登录FTP服务器。
```shell
#创建本地用户
useradd ftpuser
#设置用户密码
passwd ftpuser
```
修改`/etc/vsftpd/vsftpd.conf`
```shell
#是否允许匿名登录
anonymous_enable=NO
#是否允许本地用户登录
local_enable=YES
```
服务重启
* **vsftpd.conf的配置文件参数说明**

**用户登录相关**

参数|说明
-|-
anonymous_enable=YES|是否允许匿名用户登录。默认为YES
no_anon_password=NO|是否无需询问匿名登录用户密码。默认为NO
anon_root=/var/ftp|匿名用户登录时的目录,默认为/var/ftp。/ftp该目录不能具有777权限,即匿名用户的根目录不能具有777权限
local_enable=YES|是否允许本地用户登录。默认为YES
local_root=/home/user|本地用户登录时,将进入设置目录,默认值为各用户的home目录
ftp_username=ftp|定义匿名登录用户名称,默认为ftp
userlist_file=/etc/vsftpd/user_list|控制用户访问的文件,记录用户名
userlist_enable=NO|是否启用userlist_file文件
userlist_deny=YES|决定userlist_file中的用户是否能访问ftp服务器。若设置为YES,则文件中的用户不能访问,NO则只有文件中的用户才能访问ftp服务器。默认值为YES。注意:/etc/vsftpd/ftpusers文件的优先级更高。黑名单中无法访问的用户则一定无法访问。

**用户权限相关**

参数|说明
-|-
write_enable=YES|是否允许登录用户有写权限(全局控制),默认为YES
file_open_mode=0666|本地用户上传文件的权限,与chmods所使用的数值相同,默认值为0666
local_umask=022|本地用户上传文件的umask,默认值为077
anon_umask=077|设置匿名登录用户新增或上传文件时的umask值。默认为077,则新建文件的对应权限为700。
anon_upload_enable=NO|匿名用户可以上传文件,前提write_enable=YES与拥有上层目录的写入权限,默认为NO
anon_mkdir_write_enable=NO|是否允许匿名用户建目录,前提同上,默认为NO
anon_other_write_enable=NO|是否允许匿名用户删除或重命名,前提同上。默认值为NO
anon_world_readable_only=YES|是否允许匿名用户下载可阅读档案,默认为YES
chown_username=ftptest|匿名上传文件(非目录)所属用户名,建议不要设置为root
chown_upload=NO|是否允许改变匿名用户上传文件(非目录)的属主。默认为NO

**目录切换相关**

参数|说明
-|-
chroot_list_enable=NO|是否启用chroot_list_file配置项指定的用户列表文件。默认值为NO
chroot_list_file=/etc/vsftpd/chroot_list|用户列表文件,控制哪些用户可以切换到用户home目录的上级目录。
chroot_local_user=NO|用于指定用户列表中的用户是否允许切换到用户home目录的上级目录。默认值为NO

**通过搭配能实现以下效果：**
① 当`chroot_list_enable=YES`并且`chroot_local_user=YES`时，在`/etc/vsftpd/chroot_list`文件中列出的用户，可以切换到其他目录；未在文件中列出的用户，不能切换到其他目录。
② 当`chroot_list_enable=YES`并且`chroot_local_user=NO`时，在`/etc/vsftpd/chroot_list`文件中列出的用户，不能切换到其他目录；未在文件中列出的用户，可以切换到其他目录。
③ 当`chroot_list_enable=NO`并且`chroot_local_user=YES`时，所有的用户均不能切换到其他目录。
④ 当`chroot_list_enable=NO`并且`chroot_local_user=NO`时，所有的用户均可以切换到其他目录。

**其他相关**

参数|说明
-|-
deny_email_enable=NO|若启用此功能,则必须提供一个文件/etc/vsftpd/banner_emails,内容为邮箱地址。若是使用匿名登录,则会要求输入邮箱地址。若邮箱地址存在文件中,则允许登录。默认值为NO
banned_email_file=/etc/vsftpd/banner_emails|此文件用于记录允许登录的邮箱地址,只有在deny_email_enable=YES,才会使用此文件
message_file=.message|设置目录消息文件。默认值为.message
dirmessage_enable=YES|如果启动,则用户第一次进入某一目录时,会检查该目录下是否有message_file指定的文件。如果存在，则欢迎语显示文件内容。默认值为YES
banner_file=/etc/vsftpd/banner|当使用者登录时，会显示此设定文件的内容。通常为欢迎语或说明,默认值无。
ftpd_banner=Welcome to My FTP Server|这里用于定义欢迎字符串,banner_file是文件形式,这里是字符串形式。默认值无。
ascii_upload_enable=NO|是否启用ASCII模式上传数据。默认值为NO
ascii_download_enable=NO|是否启用ASCII模式下载数据。默认值为NO
anon_max_rate=0|设置匿名登录者的最大传输速度,单位为B/s。0表示不限速。默认值为0
local_max_rate=0|设置本地用户的最大传输速度,单位为B/s。0表示不限速。默认值为0
accept_timeout=60|设置建立FTP连接的超时时间,单位为秒。默认值为60
connect_timeout=60|PORT方式下建立数据连接的超时时间,单位为秒。默认值为120
data_connection_timeout=120|设置建立FTP数据连接的超时时间,单位为秒。默认值为120
idle_session_timeout=300|设置多长时间不对FTP服务器进行任何操作,则断开该FTP连接,单位为秒。默认值为300
xferlog_enable=YES|是否开启上传/下载日志记录。如果启动,则相关信息将被完整记录。默认值为YES
xferlog_file=/var/log/vsftpd.log|设置日志文件名和路径,默认值为/var/log/vsftpd.log
xferlog_std_format=NO|日志文件格式是否为xferlog的标准格式。默认值为NO
log_ftp_protocol=NO|启用此项,则所有的FTP请求和响应都会被记录在日志中。启用此选项,则xferlog_std_format则不能被激活。该选项用于调试。默认值为NO
user_config_dir=/etc/vsftpd/userconf|此项用于实现不同用户使用不同配置。设置用户配置文件所在目录。配置该项后,用户登录服务器,系统会自动寻找该目录下,读取与当前用户名相同的配置文件。对当前用户进行进一步配置。默认值无。
text_userdb_names=NO|设置在执行`ls -la`之类的命令时,是显示UID、GID还是显示具体的用户名和组名。默认值为NO。即以UID和GID方式显示。若希望显示用户名与组名,则设置为YES。
ls_recurse_enable=NO|若是启用,则允许登录者使用`ls -R`(可以查看当前目录下子目录中的文件)这个指令。默认值为NO。
hide_ids=NO|若是启用,则所有文件的拥有者与群组都为ftp。也就是使用者登入使用`ls -al`之类的指令,所有文件的拥有者与群组均为ftp。默认值为NO。
download_enable=YES|文件是否能下载到本地,文件夹不受影响。默认值为YES。

**FTP的工作方式与端口设置**
FTP有两种工作方式：`PORT FTP`(主动模式) 和`PASV FTP`(被动模式)

参数|说明
-|-
listen_port=21|设置FTP服务器建立连接所监听的端口。默认值为21。
connect_from_port_20=YES|指定FTP使用20端口进行数据传输。默认值为YES。
ftp_data_port=20|设置在PORT模式下,FTP数据连接使用的端口。默认值为20。
pasv_enable=YES|是否使用PASV工作模式,否则为PORT模式。默认值为YES。
pasv_max_port=0|在PASV工作模式下,数据连接可以使用的端口范围的最大端口,0表示任意端口。默认值为0。
pasv_min_port=0|在PASV工作模式下,数据连接可以使用的端口范围的最小端口,0表示任意端口。默认值为0。
listen=YES|设置vsftpd服务器是否以standalone(单机)模式运行。默认值为YES。若设置为NO,则vsftpd不是以独立服务运行,需要收到xinetd服务的管控。功能上会受到限制。
max_clients=0|设置vsftpd允许的最大连接数。0表示不受限制。默认值为0。只有在standalone模式才有效。
max_per_ip=0|设置每个IP允许与FTP服务器同时建立连接的数目。0表示不受限制。默认值为0。只有在standalone模式才有效。
listen_address=xxx.xxx.xxx.xxx|设置FTP服务器在指定的IP地址上侦听用户的FTP请求。若不设置,则对服务器绑定的所有IP地址进行侦听。只有在standalone模式才有效。
setproctitle_enable=NO|设置每个与FTP服务器的连接,是否以不同的进程表现出来。默认值为NO。此时查看vsftpd进程只有一个。若为YES,则每一个连接都有一个对应的vsftpd的进程。

**虚拟用户设置**
虚拟用户使用PAM认证方式。

参数|说明
-|-
pam_service_name=vsftpd|设置PAM使用的名称,默认值为/etc/pam.d/vsftpd
guest_enable=NO|是否启用虚拟用户。默认值为NO。
guest_username=ftp|用于映射虚拟用户。默认值为ftp。
virtual_use_local_privs=NO|当启用该项时,虚拟用户使用与本地用户相同的权限。否则与匿名用户相同权限。默认值为NO

### 6.安全组设置
ECS实例安全组需要设置入方向FTP端口放行

### 7.服务安全加固
#### 1).禁用匿名登录服务
创建一个ftp服务用户,并配置强密码
```shell
#/sbin/nologin表示该用户不能登录linux shell环境
useradd -d /home -s /sbin/nologin ftptest
#为该用户配置强密码。长度八位以上,大小写字母加数字,特殊字符
passwd ftptest
```
修改配置文件`/etc/vsftpd/vsftpd.conf`
```shell
#禁用匿名登录
anonymous_enable=NO
```
#### 2).禁止显示Banner信息
修改配置文件`/etc/vsftpd/vsftpd.conf`
```shell
#修改banner信息
ftpd_banner=Welcome
```
#### 3).限制FTP登录用户
`/etc/vsftpd/ftpusers` 属于黑名单,禁止登录ftp服务的用户。除去必要的ftp用户,其他本地用户(root、bin等)都应该添加至该文件中。
#### 4).限制FTP用户目录
修改配置文件`/etc/vsftpd/vsftpd.conf`
```shell
#开启目录访问限制
chroot_list_enable=YES
#chroot_list文件中添加的用户,只允许访问该用户的home目录
chroot_list_file=/etc/vsftpd/chroot_list
```
#### 5).修改监听地址和默认端口
```shell
listen_address=xxx.xxx.xxx.xxx
listen_port=xxxx
```
#### 6).启用日志记录
修改配置文件`/etc/vsftpd/vsftpd.conf`
```shell
#是否开启日志
xferlog_enable=YES
#是否使用xferlog标准格式
xferlog_std_format=YES
#日志位置,若目录不存在需要手动创建
xferlog_file=/var/log/ftplog
```
#### 7).其他安全设置
修改配置文件`/etc/vsftpd/vsftpd.conf`
```shell
#限制最大连接数
max_clients=100
#限制每个IP可以创建连接的连接数
max_per_ip=5
#限制传输速度,单位B/s
anon_max_rate=81920
local_max_rate=81920
```

## 四.Redis 安装
## 五.RabbitMQ 安装
## 六.Nginx 安装
### 1.安装
#### 1)环境安装
Nginx源码编译依赖gcc环境,若无gcc环境,需要运行以下命令先行安装
```shell
yum install -y gcc-c++
```
PCRE pcre-devel安装,nginx的http模块使用pcre来正则解析。
```shell
yum install -y pcre pcre-devel
```
zlib库提供了多种压缩和解压方式,nginx使用zlib对http包的内容进行gzip
```shell
yum install -y zlib zlib-devel
```
nginx提供了https协议,需要安装open-ssl
```shell
yum install -y openssl openssl-devel
```
#### 2)源码安装
[官网下载](https://nginx.org/en/download.html)源码或者
**wget下载**
```shell
wget -c https://nginx.org/download/nginx-1.14.2.tar.gz
```
**解压**
```shell
tar -zxf nginx-1.14.2
```
**默认配置**
```shell
#如果要支持https,需要增加https模块
#./configure --with-http_ssl_module
cd nginx-1.14.2
./configure
```
**自定义配置**
```shell
#需要先创建文件夹
cd nginx-1.14.2
./configure \
--prefix=/usr/local/nginx \
--conf-path=/usr/local/nginx/conf/nginx.conf \
--pid-path=/usr/local/nginx/conf/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi
```
**编译安装**
```shell
# cd nginx-1.14.2
make
make install
#查找安装路径
whereis nginx
#/usr/local/nginx
```
**创建软连接**
```shell
ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx
```
**命令**
```shell
#设置软连接后,启动
nginx
#关闭 stop:此方法=查找进程id,kill 9 杀死进程 quit:等待nginx进程任务处理完后停止
nginx -s stop
nginx -s quit
#重启 reload = nginx -s quit -> nginx
nginx -s reload
```
**开机自启**
```shell
#在rc.local中添加自启
vim /etc/rc.local
#新增一行
/usr/local/nginx/sbin/nginx
#保存后,设置执行权限
chmod 755 /etc/rc.local
```
#### 3)Https
**检测nginx是否支持https**
```shell
nginx -V
#nginx version:nginx/1.14.2
#build by gcc 4.8.5 
#configure arguments:
```
检查`configure arguments:`是否包含了-with-http_ssl_module模块,如果没有则需要重新编译。找到之前安装Nginx时的编译目录,配置ssl模块。
```shell
./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-http_stub_status_module
make
#将初始执行命令备份
mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.copy
#将新编译后的执行脚本复制到sbin/下，代替原脚本
cd objs/
cp nginx /usr/local/nginx/sbin/nginx
#升级
cd ..
make upgrade
#查看模块是否安装成功
nginx -V
```
**Https配置**
修改配置文件
```shell
cd /usr/local/nginx/conf
vim nginx.conf
```
**配置内容：**
```shell
 #HTTPS server

 server {
     listen       443 ssl; #开启ssl
     server_name  localhost;  #域名

     ssl_certificate      cert.pem; #SSL证书文件路径,由证书签发机构提供 
     ssl_certificate_key  cert.key; #SLL密钥文件路径,由证书签发机构提供

     ssl_session_cache    shared:SSL:1m;
     ssl_session_timeout  5m;

     ssl_ciphers  HIGH:!aNULL:!MD5;
     ssl_prefer_server_ciphers  on;

     location / {
         root   html;
         index  index.html index.htm;
     }
 }

```
## 七.GitLab 安装
## 八.SVN 安装
## 九.Git+Node.js安装
(https://blog.csdn.net/coding01/article/details/80083033)
## 十.用户权限控制





































