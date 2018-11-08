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



-------

[TOC]








