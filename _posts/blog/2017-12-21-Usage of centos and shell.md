---
layout: post
title: " Usage of centos and shell"
modified:
categories: blog
excerpt:
tags: [shell,mysql,linux]
image:
  feature:
date: 2017-12-21T10:08:50-04:00
modified: 2017-12-21T10:09:19-04:00
---


I used to remotely logging linux system and operate mysql temporarily. Meanwhile i have forgot how to operate it,thus i writed this article to remeber the way i used to use.

### 1.Remote login linux in ssh

```

ssh herry@43.255.154.109

```

### 2.uploading and downloading file to remote sever in ssh

```
scp /filepath/filename herry@43.255.154.109:home/www

scp -P 6666 /filepath/filename herry@43.255.154.109:home/www

scp -r /filepath/all_those_file_in_this_path herry@43.255.154.109:home/www

scp herry@43.255.154.109:home/www/filename /Users/herry/Desktop 


```


### 3.vim usage

```
 # find : 
 
 /filename
 
 # next :
 
 n
 
 # go to the head of line : 
 
 o
 
# go to line assined :  

:n 
# eg: 
:7
 
 # go to end of line: 
 Shift+A
 
 # go to last line of file: 
 
 Shift+G  or $
 
 # previous: 
 
 N
 
 # delete : 
 
 dd
 
 # input : 
 
 a
 
 # save : 
 
 esc + : qw
 
 # quit : 
 
 esc + : q
 
 # quit forcely : 
 
 esc + : q!

        
```

### 4. linux usage

```

# look files in current path: 
ls

#look authority of files: 
ll

#look all files :
ls -a

# copy directory with files to another directory

cp -rp /home/d001 /home/Documents

# -r all files ， -p retain original attribute

# delete a file

rm filename

# rename a file

mv orname curname

# rm a directory

rm -rf dirname


```

### 5. curl usage

```

# via -o/-O save file to directory assiged

# download file and named mygettext.html

curl -o mygettext.html http://www.gnu.org/software/gettext/manual/gettext.html

# download file and named gettext.html

curl -O http://www.gnu.org/software/gettext/manual/gettext.html

# get two files at the same time

curl -O URL1 -O URL2

#  via -limit-rate limite speed of downloading in 1000B/second

curl --limit-rate 1000B -O http://www.gnu.org/software/gettext/manual/gettext.html

# upload file myfile.txt to ftp sever

curl -u ftpuser:ftppass -T myfile.txt ftp://ftp.testserver.com

# upload multiple files

curl -u ftpuser:ftppass -T "{file1,file2}" ftp://ftp.testserver.com

# get content to save directory in sever

curl -u ftpuser:ftppass -T - ftp://ftp.testserver.com/myfile_1.txt

# save cookie infomation to sugarcookies

curl -D sugarcookies http://localhost/sugarcrm/index.php

# use cookie saved last time

 curl -b sugarcookies http://localhost/sugarcrm/index.php
 
# GET

curl -u username https://api.github.com/user?access_token=XXXXXXXXXX
 
# POST

curl -u username --data "param1=value1&param2=value" https://api.github.com

curl localhost:9999/api/daizhige/article -X POST -d "title=comewords&content=articleContent"

curl localhost:9999/api/daizhige/article -X POST -H "Content-Type:application/json" -d '"title":"comewords","content":"articleContent"'

# upload assigned directory with files to sever

curl --data @filename https://github.api.com/authorizations

# if special characters in post method

curl -d "value%201" http://hostname.com

# new version

curl --data-urlencode "value 1" http://hostname.com
 
# other method

curl -I -X DELETE https://api.github.cim

# upload file 

curl --form "fileupload=@filename.txt" http://hostname/resource

curl localhost:8000/api/v1/upimg -F "file=@/Users/fungleo/Downloads/401.png" -H "token: 222" -v
       
```


### 6. solution of  shbad interpreter

```

vi xxx.sh


:set ff ?

if it has occurance of  fileforma＝dos 

:set fileformat=unix

:wq

OK。。。。。。。

```


### 7. searching file according to contents


```

find -type f -name '*.php'|xargs grep 'test'


```



-------

[TOC]








