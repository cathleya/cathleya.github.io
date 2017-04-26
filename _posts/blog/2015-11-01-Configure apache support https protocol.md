---
layout: post
title: "Configure Apache support https protocol"
modified:
categories: blog
excerpt:
tags: [Apache Https]
image:
  feature:
date: 2015-11-01T15:39:55-04:00
modified: 2015-11-21T14:19:19-04:00
---


This [guide](https://getgrav.org/blog/macos-sierra-apache-ssl) is intended for your local site setup under SSL (e.g. https://yoursite.com). There are a few steps that are needed to accomplish this with your  Apache setup. 




## 1. The first step is to make some modifications to your httpd.conf:

```
sudo vi /etc/apache2/httpd.conf
```
In this file you should uncomment both the socache_shmcb_module, ssl_module, and also the include for the httpd-ssl.conf by removing the leading # symbol on those lines:

```
LoadModule socache_shmcb_module libexec/mod_socache_shmcb.so
...
LoadModule ssl_module libexec/mod_ssl.so
...
Include /usr/local/etc/apache2/2.4/extra/httpd-ssl.conf


```

## 2. After saving this file, you should then open up your /usr/local/etc/apache2/2.4/extra/httpd-vhosts.conf to add appropriate SSL based virtual hosts.

```
sudo vi /etc/apache2/extra/httpd-vhosts.conf
```

Here you can create a VirtualHost entry for each virtual host that you wish to provide SSL support for.

```
<VirtualHost *:443>
    DocumentRoot "/Users/your_user/Sites"
    ServerName localhost
    SSLEngine on
    SSLCertificateFile "/usr/local/etc/apache2/2.4/server.crt"
    SSLCertificateKeyFile "/usr/local/etc/apache2/2.4/server.key"
</VirtualHost>

```

In this example we have created the VirtualHost for localhost, but it could be any of your existing or even a new VirtualHost. The important parts are the the 443 port, along with SSLEngine on and the SSLCertificateFile and SSLCertificateKeyFile entries that point to the certificate


## 3. To get this all to work with Apache, we need to create a self-signed certificate that we have already referenced in the VirtualHost definition.

```
$ cd /etc/apache2
$ sudo ssh-keygen -f server.key
Then generate a certificate signing request:

$ sudo openssl req -new -key server.key -out server.csr
Using this CSR, generate the certificate:

$ sudo openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
The convert the key to a no-phrase key:

$ sudo openssl rsa -in server.key -out server.key
Then all you need to do now is double check your Apache configuration syntax:

$ sudo apachectl configtest
```

If all goes well, restart Apache:

```
$ sudo apachectl -k restart
```




## 4. changing media type 

```
sudo vi /etc/apache2/mime.types

application/octet-stream ipa text/xml plist

```



-------

[TOC]

