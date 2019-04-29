## 创建用户，授权

创建

```bash
mysql -u root -p
#密码

#插入用户
xx> CREATE USER 'test'@'%' IDENTIFIED BY 'password';
xx> exit

#test
mysql -u test -p

```

授权

```bash
mysql -u root -p
#密码

xx> create database testDB;

xx> grant all privileges on testDB.* to test identified by '1234';

xx> flush privileges;

#所有数据库权限
## mysql>grant select,delete,update,create,drop on *.* to test@"%" identified by "1234";


```

## 其他常见操作

```bash
#删除用户
mysql>Delete FROM user Where User='test' and Host='localhost';
mysql>flush privileges;
mysql>drop database testDB; //删除用户的数据库
#删除账户及权限：
mysql>drop user 用户名@'%';
mysql>drop user 用户名@ localhost; 
#修改指定用户密码
mysql>update mysql.user set password=password('新密码') where User="test" and Host="localhost";
mysql>flush privileges;
#列出所有数据库
mysql>show database;
#切换数据库
mysql>use '数据库名';
#列出所有表
mysql>show tables;
#显示数据表结构
mysql>describe 表名;
#删除数据库和数据表
mysql>drop database 数据库名;
mysql>drop table 数据表名;
```
