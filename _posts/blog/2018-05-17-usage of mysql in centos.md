---
layout: post
title: "Importing mysql file to centos"
modified:
categories: blog
excerpt:
tags: [centos,mysql]
image:
  feature:
date: 2018-05-17T10:08:50-04:00
modified: 2018-05-17T10:09:19-04:00
---


This blog records how i use mysql in centos.


### Feature ###

* usage of msyql,centos
* importing mysql file

### steps ###

1. uploading mysql file to remote centos server
    

 ```
        ssh herry@43.255.154.109

        scp /filepath/filename herry@43.255.154.109:home/www     
```


2. access mysql

```

    mysql -u root -p

```

3. create and use database

```

    create database Student

    use Student

    set names utf8

    show databases;

    drop database Student;

```

----------

```

mysql -u root -p
>password:

mysql>use yourdatabasename;
mysql>set names utf8;
mysql>source /tmp/database.sql;


```


4. import myql

```
    source /developer/Student.sql;
```

5. grant local terminal to access remote mysql database

```
    GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'youpassword' WITH GRANT OPTION;

    flush privileges;
```

    ps:%->local ip

6. disposal error access deny

    open mysql.conf comment 

    bind-address           = 127.0.0.1


### 6.mysql usage in centos 7


# 1. download mysql repo 

```

wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm

```


# 2. install mysql-community-release-el7-5.noarch.rpm package

```

sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm

```

# 3. install mysql

```

sudo yum install mysql-server

```

# 4. reset mysql password

```

mysql -u root

```

# ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)，

```

sudo chown -R root:root /var/lib/mysql

```

# restart mysql sever


```

service mysqld restart

```

# login and reset password

```

mysql -u root  
mysql > use mysql;
mysql > update user set password=password('123456') where user='root';
mysql > exit;

```


# MYSQL create mysql database in utf-8 format

# GBK: 

```

create database test2 DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci; 

``` 

# UTF8: 

```
CREATE DATABASE `test2` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;  

mysql -u root -p 
show databases;


CREATE DATABASE `test` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;  

Query OK, 1 row affected (0.06 sec) 

```


# set database in utf-8 format

```

rows in set (0.07 sec) 


```


# solution of that Host is not allowed to connect to this MySQL server



 ```

MySQL mysql -u root -p

use mysql;

update user set host = '%' where user = 'root';

ignorance of this sentence

FLUSH PRIVILEGES;



 ```



-------

[TOC]








