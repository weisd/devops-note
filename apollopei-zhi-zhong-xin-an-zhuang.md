
# apollo安装

### 下载安装包


从[GitHub Release](https://github.com/ctripcorp/apollo/releases)页面下载最新版本的apollo-configservice-x.x.x-github.zip、apollo-adminservice-x.x.x-github.zip和apollo-portal-x.x.x-github.zip


解压对应包

安装mysql 
wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
yum localinstall mysql57-community-release-el7-11.noarch.rpm
yum install -y mysql-community-server

systemctl start mysqld

systemctl enable mysqld

grep 'temporary password' /var/log/mysqld.log

ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';

### 初始化数据库
Apollo服务端共需要两个数据库：ApolloPortalDB和ApolloConfigDB，我们把数据库、表的创建和样例数据都分别准备了sql文件，只需要导入数据库即可
https://github.com/ctripcorp/apollo/blob/master/scripts/db/migration/portaldb/V1.0.0__initialization.sql

https://github.com/ctripcorp/apollo/blob/master/scripts/db/migration/configdb/V1.0.0__initialization.sql

curl https://raw.githubusercontent.com/ctripcorp/apollo/master/scripts/db/migration/portaldb/V1.0.0__initialization.sql -o  portaldb.sql

curl https://raw.githubusercontent.com/ctripcorp/apollo/master/scripts/db/migration/configdb/V1.0.0__initialization.sql -o configdb.sql

// 创建ApolloPortalDB和ApolloConfigDB数据库
create database ApolloPortalDB;
create database ApolloConfigDB;

// 导入数据库
use ApolloPortalDB;
source /data/apollo/portaldb.sql;
use ApolloConfigDB;
source /data/apollo/configdb.sql;

wget  https://github.com/vrana/adminer/releases/download/v4.7.3/adminer-4.7.3-mysql.php

https://wiki.archlinux.org/index.php/Adminer#Nginx
https://wiki.archlinux.org/index.php/Nginx#FastCGI

安装php mysql扩展
yum -y install php-mysql
yum -y install php-fpm
systemctl restart php-fpm
systemctl enable php-fpm


adminert-zjjy.ktkt.com



创建数据库用户
修改连接数据库配置
注意账号密码前后不要有空格！

vi adminservice/config/application-github.properties

vi configservice/config/application-github.properties

vi portal/config/application-github.properties

修改 portal env
vi portal/config/apollo-env.properties

dev.meta=http://localhost:8080


SHOW VARIABLES LIKE 'validate_password%';
set global validate_password_policy=0;
set global validate_password_length=4;
mysql_secure_installation

CREATE USER 'apollo'@'10.%' IDENTIFIED BY 'x123.XX!’;
GRANT ALL ON ApolloConfigDB.* TO 'apollo'@'10.%';
GRANT ALL ON ApolloPortalDB.* TO 'apollo'@'10.%';
flush privileges;

CREATE USER 'apollo'@'localhost' IDENTIFIED BY 'x123.XX!';
GRANT ALL privileges ON ApolloConfigDB.* TO 'apollo'@'localhost';
GRANT ALL privileges ON ApolloPortalDB.* TO 'apollo'@'localhost';
flush privileges;

安装java

yum -y install java

启动
记得在scripts/startup.sh中按照实际的环境设置一个JVM内存，以下是我们的默认设置，供参考：

8080
./configservice/scripts/startup.sh
8090
./adminservice/scripts/startup.sh
8070
./portal/scripts/startup.sh

安装nginx,添加代理配置
```
server {
        listen 80;
        server_name apollot.ktkt.com;

        location / {

        proxy_set_header Host $host;
        proxy_set_header  X-Real-IP        $remote_addr;
        proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;

        proxy_pass http://localhost:8070;

        }

}
```
修改默认账号密码apollo/admin
修改部门列表：organizations


CREATE DATABASE `zjjy` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE USER ‘zjjy’@’10.%' IDENTIFIED BY ‘x123.XX!’;
grant all privileges on zjjy.* to 'zjjy’@’10.%’;
drop user zjjy@'%';