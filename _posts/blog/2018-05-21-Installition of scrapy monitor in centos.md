---
layout: post
title: "Installition of scrapy monitor in centos"
modified:
categories: blog
excerpt:
tags: [scrapy scrapydweb]
image:
  feature:
date: 2018-05-21T10:08:50-04:00
modified: 2018-05-21T10:09:19-04:00
---


This blog records how i used.


### Feature ###

* 
*

1. Installition of Anaconda

```
	cd ~

	wget https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh

	bash Anaconda3-5.3.1-Linux-x86_64.sh 

	source /root/.bashrc

	conda create -n pyprojects

	conda activate pyprojects

	conda deactivate pyprojects

```

2. Installition of scrapyd

```
	conda install scrapyd

	requests pymysql

```

3. Installition of scrapydweb


```
	
	pip install scrapydweb

	scrapydweb -h  生成配置文件

```


4. edting configuration of scrapydweb


```

	SCRAPYDWEB_BIND = '127.0.0.1'

	ENABLE_AUTH = True

	USERNAME = 'username'
	PASSWORD = 'password'

	SCRAPY_PROJECTS_DIR = '/www/scprojects'

```

5. running scrapyd and scrapydweb in background

```
	nohup scrapyd > /www/logs/scrapyd.log & 
	
	nohup scrapydweb > /home/www/logs/scrapydweb.log & 

	nohup scrapydweb > /home/www/logs/scrapydweb.log 2>&1 &

```

6. proxy of nginx server

```
    server{
        listen 80;
		#ui
        location / {
            proxy_pass     http://127.0.0.1:5000/;    # 反向代理的服务地址
            auth_basic     "Restricted";  # 表示限制访问    
            auth_basic_user_file    /etc/nginx/conf.d/.htpasswd;  # 认证配置文件路径
            }
        }
		#logs
		location logs/ {
            proxy_pass     http://127.0.0.1:6800/;
            auth_basic     "Restricted"; 
            auth_basic_user_file    /etc/nginx/conf.d/.htpasswd;
            }
        }
    }

```

7. configuration of starting scrapyd automatically


```

/root/anaconda3/envs/pyprojects/bin/scrapyd

/root/anaconda3/envs/pyprojects/bin/scrapydweb

vi /etc/init.d/scrapyd

```

----------

```

	#!/bin/bash
	PORT=6800
	HOME="/var/scrapyd"
	BIN="/root/anaconda3/envs/pyprojects/bin/scrapyd"
 
	pid=`netstat -lnopt | grep :$PORT | awk '/python/{gsub(/\/python/,"",$7);print $7;}'`
	start() {
   		if [ -n "$pid" ]; then
      		echo "server already start,pid:$pid"
      		return 0
   		fi
 
   		cd $HOME
   		nohup $BIN >> $HOME/scrapyd.log 2>&1 &
   		echo "start at port:$PORT"
	}
 
	stop() {
   		if [ -z "$pid" ]; then
      		echo "not find program on port:$PORT"
     		return 0
   		fi
 
   		#结束程序，使用讯号2，如果不行可以尝试讯号9强制结束
   		kill -9 $pid
   		echo "kill program use signal 9,pid:$pid"
	}
 
	status() {
   		if [ -z "$pid" ]; then
      		echo "not find program on port:$PORT"
   		else
      		echo "program is running,pid:$pid"
   		fi
	}
 
	case $1 in
   		start)
      		start
   		;;
   		stop)
      		stop
   		;;
   		status)
      		status
   		;;
   		*)
      		echo "Usage: {start|stop|status}"
   		;;
	esac
 
	exit 0

```

8. new log directory

/var/scrapyd

9. running scrapyd

```
service scrapyd {start|stop|status} 
```

10. running scrapydweb

```
	conda activate pyprojects

	nohup scrapydweb > /home/www/logs/scrapydweb.log & 

	nohup scrapydweb > /home/www/logs/scrapydweb.log 2>&1 &

```













-------

[TOC]








