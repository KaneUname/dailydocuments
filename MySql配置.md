# 安装MySql

## 1、下载

```shell
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.37-linux-glibc2.12-x86_64.tar.gz
```

## 2、解压并修改名称

```shell
tar -zxvf mysql-5.7.37-linux-glibc2.12-x86_64.tar.gz -C /usr/local/

mv mysql-5.7.37-linux-glibc2.12-x86_64/ mysql
```

## 3、创建 用户，并给数据目录赋予权限

```shell
# 创建mysql组和用户
groupadd mysql
useradd -r -g mysql mysql

#创建mysq数据目录
#回到根目录
cd /
mkdir -p data
cd data/
mkdir -p mysql

#赋予权限
chown mysql:mysql -R /data/mysql
```

## 4、配置参数

```shell
vim /etc/my.cnf

[mysqld]
bind-address=0.0.0.0
port=3306
user=mysql
basedir=/usr/local/mysql
datadir=/data/mysql
socket=/tmp/mysql.sock
log-error=/data/mysql/mysql.err
pid-file=/data/mysql/mysql.pid
#character config
character_set_server=utf8mb4
symbolic-links=0
```

## 5、初始化mysql

```shell
cd /usr/local/mysql/bin/

./mysqld --defaults-file=/etc/my.cnf --basedir=/usr/local/mysql/ --datadir=/data/mysql/ --user=mysql --initialize

#查看初始密码，复制出来
vim /data/mysql/mysql.err
```

## 6、 将二进制安装的MySQL纳入systemd管理

```shell
vim /etc/systemd/system/mysqld.service

[Unit]
Description=MySQL Server
After=network.service
[Install]
WantedBy=multi-user.target
[Service]
User=mysql
Group=mysql
Type=forking
TimeoutSec=0
PermissionsStartOnly=true
ExecStart=/usr/local/mysql/bin/mysqld --defaults-file=/etc/my.cnf --daemonize
LimitNOFILE=5000
Restart=on-failure
RestartSec=10
RestartPreventExitStatus=1
PrivateTmp=false
```

## 6、启动mysql，并更改root 密码

```shell
# 通过systemctl启动mysqld.service
systemctl daemon-reload
systemctl start mysqld
systemctl status mysqld
#配置开机自启动
systemctl enable mysqld
```

## 7、登录mysql 修改密码，访问权限

```shell
# 登录mysql 修改密码，访问权限
./mysql -hlocalhost -uroot -p

# 修改密码
set password=password('wk123456');
flush privileges;

# 修改访问权限
use mysql;
update user set Host='%' where User='root';
flush privileges;

# 远程连接连不上时使用授权法
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'wk123456' WITH GRANT OPTION;
flush privileges;
```

