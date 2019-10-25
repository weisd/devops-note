# 安装mysql5.7

Contos7 后mysql被mariadb代替，默认yum源安装的mysql是mariadb, 如果想安装mysql, 要自己动手

下载rpm

```shell
wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
```

本地安装源

```
yum localinstall mysql57-community-release-el7-11.noarch.rpm
```

安装mysql
```
yum install -y mysql-community-server
```

启动服务
```
yum install -y mysql-community-server
```

加入开机启动
```
systemctl enable mysqld

```

数据库安装完后，默认会给root创建一个密码，可以在log里找到
```
grep 'temporary password' /var/log/mysqld.log
```

用查到的密码登陆
```
mysql -u root -p
```

修改root密码
```
grep 'temporary password' /var/log/mysqld.log
```