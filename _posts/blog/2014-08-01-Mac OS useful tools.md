---
layout: post
title: "Mac OS useful tools"
modified:
categories: blog
excerpt:
tags: [Useful Tools]
image:
  feature:
date: 2014-08-01T15:39:55-04:00
modified: 2016-06-01T14:19:19-04:00
---



# mac os useful tools

## 1. make mac os launching USB drive and restall mac os

1.luanch disk utility.app
2.erase disk(format:log)
3.modify disk name
4.download mac os from appstore 
5.launch terminal
6.type below order
`sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/Herry --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app --nointeraction`

7.restart mac ,keeping pressing option key
8.select usb launch
9.launch disk utility.app
10.erase all mac disk
11.quit system ,realize clean installation of mac os 

#####Sierra
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/Sierra --applicationpath /Applications/Install\ macOS\ Sierra.app --nointeraction

## 2. terminal order set mac os shut down|sleep|restart automatically

1.set mac os shut down at 2013.11.4 14:10

`sudo shutdown -h 1311041410 | sudo shutdown -h 14:10`

2.set mac os restart at 2013.11.4 14:10

`sudo shutdown -r 1311041410 | sudo reboot |sudo shutdown -r now`

3.set mac os sleep at 2013.11.4 14:10

`sudo shutdown -s 1311041410`

4.if you wanna shut down/restart/sleep at once

`sudo halt | sudo shutdown -h now`

5.cancel above operation temporarily
    For example you typed :
    
    sudo shutdown -s 1311041410
    Shutdown at Mon Nov  4 14:10:00 2013.
    shutdown: [pid 6609]
    
-------
then you wanna give up shutting down ,then type:
    


    sudo kill 6609
    
    
## 3. MAC os remote connect windows disktop     

1.dowload and install [Remote Desktop Connection](http://www.microsoft.com/en-us/download/details.aspx?id=18140)

2.launch Remote Desktop Connection and input ip/computer name &&usr name and password


## 4. MAC os apache macos-sierra-apache-ssl

[vist blog here for more detail](https://getgrav.org/blog/macos-sierra-apache-ssl)

1.$ sudo nano /etc/apache2/httpd.conf

2.LoadModule socache_shmcb_module libexec/apache2/mod_socache_shmcb.so
...
LoadModule ssl_module libexec/apache2/mod_ssl.so
...
Include /private/etc/apache2/extra/httpd-ssl.conf

3.$ sudo nano /etc/apache2/extra/httpd-vhosts.conf

<VirtualHost *:443>
DocumentRoot "/Users/your_user/Sites"
ServerName localhost
SSLEngine on
SSLCertificateFile "/private/etc/apache2/server.crt"
SSLCertificateKeyFile "/private/etc/apache2/server.key"
</VirtualHost>

First generate a key:

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
If all goes well, restart Apache:

$ sudo apachectl -k restart

***please draw attention to media type***
/private/etc/apache2/mime.types

application/octet-stream ipa
text/xml plist

## 4. MAC os useful apps 

1. Do Http
2. UnRarX
3. SiteSucker
4. GifBrewery
5. holaVPN
6. Charles
7. VLC
8. CheatSheet
9. sip
10. Easy APNs Provider
11. APNs Tester
12. MWeb
13. Reveal
14. SourceTree
15. MacAutoDMG
16. Navicate for MySql
17. sublime text
18. PhpStorm
19. WebStorm
20. Animate CC
21. [ErlangInstaller](http://rudix.org/packages/erlang.html)
22. Parallels Desktop 
23. TeamViewer
24. prepo



-------

[TOC]

