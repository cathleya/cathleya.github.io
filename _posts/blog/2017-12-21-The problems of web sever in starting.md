---
layout: post
title: "The problems of web sever in starting"
modified:
categories: blog
excerpt:
tags: [php,mysql,phpMyAdmin]
image:
  feature:
date: 2017-12-24T10:12:30-04:00
modified: 2017-12-24T22:09:49-04:00
---


Today I've putted my sample web project to sever,there are so many problems occured on my way,here is the disposal.


### Access denied for user 'root' in mysql ###

* install php-mysql extension,cuz php71 installitation without mysql

    `sudo yum install php71w-mysql
     systemctl restart php-fpm `

* open 3306 port

    `cd /etc/sysconfig
     sudo vi iptables
     systemctl restart iptables`

* install [phpMyAdmin](https://www.phpmyadmin.net/downloads/)

	 `sudo chmod -R 0777 /var/lib/php/session`
	
    `vi phpMyAdmin/libraries/config.default.php `
    
    `$cfg['Servers'][$i]['AllowNoPassword'] = true`
	
	

### The disposal of error ###

* Error in (sudo yum install php-mysql)

    `Error: php71w-common conflicts with php-common-5.4.16-43.el7_4.x86_64`

* disposal

    `sudo yum install php71w-mysql`

* Warning: mysql_connect(): Headers and client library minor version mismatch.

   `yum remove php-mysql  
   yum install php-mysqlnd `
   
* CI cathepa

       `yum install php71w-gd
       systemctl restart php-fpm`
    
    


-------

[TOC]








