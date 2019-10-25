# 安装mysql5.7

Contos7 后mysql被mariadb代替，默认yum源安装的mysql是mariadb, 如果想安装mysql, 要自己动手



```shell
wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm

yum localinstall mysql57-community-release-el7-11.noarch.rpm

yum install -y mysql-community-server
```