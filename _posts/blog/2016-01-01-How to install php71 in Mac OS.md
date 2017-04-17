---
layout: post
title: "How to install php71 in Mac OS"
modified:
categories: blog
excerpt:
tags: [php71]
image:
  feature:
date: 2015-08-11T08:08:50-04:00
---


# php71-install 

## install brew 

http://brew.sh/


```
brew list
brew unlink php56
brew link php55
brew install php-version
source $(brew --prefix php-version)/php-version.sh
php-version
php-version 5.5.15

```

## how to install php71

****************************
```
brew tap homebrew/dupes

brew tap homebrew/versions

brew tap homebrew/homebrew-php

brew install php71

brew services start php71

brew install nginx

sudo nginx -s reload

ps -ef|grep php

sudo kill -INT 13213

```

*********************************

The php.ini file can be found in:
/usr/local/etc/php/7.1/php.ini


muti version :

```
brew install php-version

source $(brew --prefix php-version)/php-version.sh
```


## delete origional apache 

restart mac 

keepping pressing command + R .thus,enter recovery model

open termial type :csrutil disable

(csrutil enable)

```

sudo rm -rf /etc/apache2

sudo rm -rf /usr/include/apahce2

```

## delete origional php

```

sudo rm -rf /usr/php

sudo rm -rf /usr/bin/php

sudo rm -rf /usr/bin/php-config

sudo rm -rf /usr/bin/phpize

sudo rm -rf /usr/include/php

sudo rm -rf /usr/lib/php

sudo rm -rf /usr/share/man/man*/php*

```

## fix sudo: no valid sudoers sources found, quitting

cd /etc  ,i find a file named sudoers~org and sudoers,so delete sudoers and change the name of the sudoers~org to  sudoers,everything is fine.

----------------------------
## swoole installation

```
brew search php71-swoole

brew install php71-swoole

modify  php.ini ,append 

;extension=swoole.so

 
kill the php process

brew services start php71

```


## redis installation

```
brew install redis

brew install php55-redis --build-from-source

```


## mysql installation

```
brew install mysql

mysql.server start

{start|stop|restart|reload|force-reload|status}
```



-------

[TOC]








