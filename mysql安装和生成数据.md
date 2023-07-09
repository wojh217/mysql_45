# 安装

## 在线安装

``````
$ sudo apt-get install mysql-server

``````

## 设置密码和允许远程访问

``````
$ mysql -u root -p
mysql> show databases;  # 查看数据库
mysql> use mysql;       # 切换到数据库
mysql> select user,authentication_string, plugin from user; # 查看初始密码
mysql> update user set plugin='mysql_native_password' where user='root'; # 更新为密码登录形式
mysql> update user set authentication_string=password('Abc+1234') where user='root';

# grant all privileges on 数据库名.表名 to ‘用户名’@‘IP地址’ identified by ‘用户密码’ with grant option;
mysql> grant all privileges on *.* to root@'%' identified by '123456' WITH GRANT OPTION; # 允许远程访问
mysql> flush privileges;  # 刷新配置
mysql> exit
``````

修改配置文件*/etc/mysql/mysql.conf.d/mysqld.cnf*，将bind-address改为0.0.0.0



# 插入测试数据

## 下载脚本

项目地址：[mysql_tester](https://github.com/wuda0112/mysql-tester)

``````
>mysql source /tmp/mysql_tester.sql
``````

## 运行命令

``````
$ java -jar mysql-tester-1.0.4.jar 
--mysql-username root 
--mysql-password Abc+1234 
--mysql-url "jdbc:mysql://localhost:3306/?useSSL=false&serverTimezone=UTC"  
--user-count 1000000
``````



## 查看大小

``````
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| foundation_commons |
| foundation_item    |
| foundation_store   |
| foundation_user    |
| mysql              |
| performance_schema |
| sys                |
+--------------------+

mysql> use information_schema; # 这个数据库存储大小信息

# 所有大小
mysql> select concat(round(sum(data_length/1024/1024),2),'MB') as data from tables;
+-----------+
| data      |
+-----------+
| 3789.31MB |
+-----------+

# item库大小
mysql> select concat(round(sum(data_length/1024/1024),2),'MB') as data from tables where table_schema='foundation_item';
+-----------+
| data      |
+-----------+
| 1882.97MB |
+-----------+

# item库中item表大小
mysql> select concat(round(sum(data_length/1024/1024),2),'MB') as data from tables where table_schema='foundation_item' and table_name='item';
+----------+
| data     |
+----------+
| 410.00MB |
+----------+
``````







