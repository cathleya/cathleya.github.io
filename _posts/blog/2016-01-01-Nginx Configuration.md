---
layout: post
title: "Nginx Configuration"
modified:
categories: blog
excerpt:
tags: [Nginx,hls,rtmp]
image:
  feature:
date: 2015-06-11T08:08:50-04:00
---


If you install nginx in Mac OS ,your nginx can't work correctly,especially con't find right defined controllers and functions which correspond to rounters.The following configration can configuration can help you resolve those problem,and make it support hls protocol and rtmp protocol.


# Nginx Configuration 


## nginx.conf

```

#user  nobody staff;
worker_processes  1;
pid        /usr/local/var/run/nginx.pid;
events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    index  index.html index.htm index.php;
    autoindex on;
    include servers/*;
}


```

### servers/server1.conf

```
server {
        listen       8088;
        server_name  127.0.0.1;
        charset utf8;
        root   /Users/herry/Sites/;


        location ~* ^.+.(jpg|jpeg|gif|css|png|js|ico|xml)$ {
          expires           max;
        }

        location ~ \.php {
            root           /Users/herry/Sites/;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            include        fastcgi_params;
            set $real_script_name $fastcgi_script_name;
            if ($fastcgi_script_name ~ "^(.+?\.php)(/.+)$") {
                 set $real_script_name $1;
                 set $path_info $2;
             }
             fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
             fastcgi_param SCRIPT_NAME $real_script_name;
             fastcgi_param PATH_INFO $path_info;
         }

    }

```

If you wanna your niginx support hls & rtmp protocol,you can use the following configuration.


## nginx.conf


```

#user  herry staff;
worker_processes  1;
pid        /usr/local/var/run/nginx.pid;
events {
    worker_connections  1024;
}

rtmp {
  server {
    #这个是默认的监听端口号
    listen 1935;
    chunk_size 4000;
    timeout 10s;
    #注意这个rtmplive可以自己任意写
    application hls {
            live on;
            hls on;
            hls_path /Users/herry/Sites/rtmp;
            hls_fragment 5s;
    }
  }
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    index  index.html index.htm index.php;
    autoindex on;
    include servers/*;
}


```

###	servers/server1.conf

```

server {
        listen       8081;
        server_name  127.0.0.1;
        charset utf8,gbk;
        root   /Users/herry/Sites/;

        location /hls {
            types {
                application/vnd.apple.mpegurl m3u8;
                video/mp2t ts;
            }
            root html;
            expires -1;
        }

        location ~* ^.+.(jpg|jpeg|gif|css|png|js|ico|xml)$ {
          expires           max;
        }

        location ~ \.php {
            root           /Users/herry/Sites/;
            fastcgi_pass   127.0.0.1:9001;
            fastcgi_index  index.php;
            include        fastcgi_params;
            set $real_script_name $fastcgi_script_name;
            if ($fastcgi_script_name ~ "^(.+?\.php)(/.+)$") {
                 set $real_script_name $1;
                 set $path_info $2;
             }
             fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
             fastcgi_param SCRIPT_NAME $real_script_name;
             fastcgi_param PATH_INFO $path_info;
         }

    }

```

###	servers/servermedia.conf

```

server {
        listen       8082;

        location /stat {
			       rtmp_stat all;
			          rtmp_stat_stylesheet stat.xsl;
		    }

        location /stat.xsl {
          root /usr/local/Cellar/rtmp-nginx-module/1.1.7.10/share/rtmp-nginx-module/;
        }

    		location /control {
    			rtmp_control all;
    		}

            location /hls {
                # Serve HLS fragments
                types {
                    application/vnd.apple.mpegurl m3u8;
                    video/mp2t ts;
                }
                root html;
                expires -1;
            }



    }


```


how to test your configuration work correctly which support hls & rtmp protocol.


# nginx-hls

###### Install Homebrew

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

brew cleanup

brew doctor

brew tap homebrew/nginx

###### intstall nginx

`brew install nginx-full --with-rtmp-module`

brew info nginx-full

cd /usr/local/etc/nginx

modify nginx config

`sudo nginx -s reload`

sudo nginx -s stop

sudo php-fpm


###### install ffmpeg

`brew install ffmpeg`

ffmpeg -re -i /Users/liuhong/Movies/Demo.mov -vcodec copy -f flv rtmp://localhost:1935/rtmplive/home

`ffmpeg -re -i /Users/herry/Desktop/14564977406580.mp4 -vcodec libx264 -acodec aac -f flv rtmp://localhost:1935/hls/home`

###### examine in nginx server
cd home

mkdir rtmp

mkdir hls


###### examine in browser
http://127.0.0.1:8081/stat 

###### examine in IOS
http://localhost:8081/rtmp/home.m3u8

    import AVFoundation
    
    let container = UIView(frame: CGRect(x: 5, y: 80, width: 300, height: 200))
    container.backgroundColor = UIColor.cyan
    view.addSubview(container)
        
    let url = URL(string: "http://localhost:8081/rtmp/home.m3u8")
    let player = AVPlayer(url: url!)
        
    let layer = AVPlayerLayer(player: player)
    layer.frame = container.bounds
    container.layer.addSublayer(layer)
        
    player.play()


###### examine in VLC software
rtmp://127.0.0.1:1935/hls/home











-------

[TOC]








