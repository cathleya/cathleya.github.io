---
layout: post
title: "Nginx Configuration"
modified:
categories: blog
excerpt:
tags: [Nginx,hls,rtmp]
image:
  feature:
date: 2016-06-11T08:08:50-04:00
modified: 2017-02-11T14:19:19-04:00
---


If you install nginx on Mac OS ,your nginx won't work well,especially it can't find right defined controllers and functions which correspond to the rounters.The following configration  can help resolve those problems,and let it support hls protocol and rtmp protocol.


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



###  CI -	ignorance of index.php in the urls


```

server {
        listen       80;
        server_name  www.xally.org xally.org;
        root   "D:\phpStudy\PHPTutorial\WWW\xally";
        location / {
            index  index.html index.htm index.php;
		if (-f $request_filename) {
        		expires max;
        		break;
    	 	}
   	 	if (!-e $request_filename) {
        		rewrite ^/(.*)$ /index.php/$1 last;
    	    	}
            #autoindex  on;
        }
        location ~ \.php(.*)$ {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_split_path_info  ^((?U).+\.php)(/?.+)$;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            fastcgi_param  PATH_INFO  $fastcgi_path_info;
            fastcgi_param  PATH_TRANSLATED  $document_root$fastcgi_path_info;
            include        fastcgi_params;
        }
}


location /{
	index  index.html index.htm index.php;
    	if (-f $request_filename) {
        expires max;
        break;
    	}
   	 if (!-e $request_filename) {
        rewrite ^/(.*)$ /index.php/$1 last;
    	}
}


```


###	Judge if it's a spider jump


```

	location /
	{
		try_files $uri $uri/ /index.php?$args;
  		set $ant 0;
  		if ($http_user_agent ~* "spider|bot")
    	{
			set $ant 1;  
    	}
 		if ( $ant = 0 ){
			proxy_pass http://370zs.com;
		}
	}

```


###	Judgement of spider and proxy


```

	upstream zhizhu {
		server 192.168.2.147;
	}

	server{
		location /   {
			if ($http_user_agent ~* "qihoobot|Baiduspider|Googlebot|Googlebot-Mobile|Googlebot-Image|Mediapartners-Google|Adsbot-Google|Feedfetcher-Google|Yahoo! Slurp|Yahoo! Slurp China|YoudaoBot|Sosospider|Sogou spider|Sogou web spider|MSNBot|ia_archiver|Tomato Bot|YisouSpider") 
			{ 
				proxy_pass http://zhizhu;

			} 
		}
	}

```


-------

[TOC]








