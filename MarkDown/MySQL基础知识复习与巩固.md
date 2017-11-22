# MySQL基础知识复习与巩固
## 一  Linux安装MySQL
先要检查Linux系统中是否已经安装了MySQL，输入命令尝试打开MySQL服务：

```
sudo service mysql start
```
输入密码后，如果出现以下提示，则说明系统中已经安装有 MySQL：
![image](https://dn-anything-about-doc.qbox.me/MySQL/sql-01-01-.png/logoblackfont)

如果提示是这样的，则说明系统中没有 MySQL，需要继续安装：

```
mysql: unrecognized service
```

在Ubuntu上安装MySQL，最简单的方式是在线安装。只需要几行简单的命令（ # 号后面是注释）：

```
#安装 MySQL 服务端、核心程序
sudo apt-get install mysql-server

#安装 MySQL 客户端
sudo apt-get install mysql-client
```

在安装过程中会提示确认输入YES，设置 root 用户密码（之后也可以修改）等，稍等片刻便可安装成功。

安装结束后，用命令验证是否安装并启动成功：
![image](https://dn-anything-about-doc.qbox.me/MySQL/sql-01-02.png/logoblackfont)

此时，可以根据自己的需求，用 gedit 修改 MySQL 的配置文件（my.cnf）,使用以下命令:


```
sudo gedit /etc/mysql/my.cnf
```

至此，MySQL 已经安装、配置完成，可以正常使用了。

使用如下两条命令，打开MySQL服务并使用root用户登录：
![image](https://dn-anything-about-doc.qbox.me/MySQL/sql-01-03-.png/logoblackfont)


## 二 数据库的增删改操作

使用命令 show databases;，查看有哪些数据库（注意不要漏掉分号 ;）：
![image](https://dn-anything-about-doc.qbox.me/MySQL/sql-01-04.png/logoblackfont)

选择连接其中一个数据库，语句格式为 use <数据库名>，这里可以不用加分号，这里我们选择 information_schema 数据库：
![image](https://dn-anything-about-doc.qbox.me/MySQL/sql-01-05.png/logoblackfont)

使用命令 show tables; 查看数据库中有哪些表（注意不要漏掉“;”）：
![image](https://dn-anything-about-doc.qbox.me/MySQL/sql-01-06.png/logoblackfont)

使用命令 quit 或者 exit 退出 MySQL。


```
CREATE DATABASE mysql_shiyan;

use mysql_shiyan

show tables

CREATE TABLE employee (id int(10),name char(20),phone int(12));

INSERT INTO employee(id,name,phone) VALUES(01,'Tom',110110110);

INSERT INTO employee VALUES(02,'Jack',119119119);

INSERT INTO employee(id,name) VALUES(03,'Rose');

SELECT * FROM employee;


```

三 参考链接
1. [MySQL 基础课程](https://www.shiyanlou.com/courses/9)


