---
layout: post
title: nginx配置反向代理，实现Restful
date: 2018-04-19 
tag: nginx
---


Nginx结合tomcat配置反向代理多个Server
---
由于项目的积累，现阶段的web开发已经逐渐趋向于Restful的开发模式，实现前后端的分离，将耦合性最大化。而现阶段多使用nginx的反向代理来解决前后端的跨域问题。
---

简单的介绍了一下如何利用nginx结合tomcat进行使用，达到反向代理的作用。现在我们要使用nginx达到这样的一个目的，能够代理多个服务器。主要是对conf里面server的配置


```
server {
        listen       8082;
        server_name  firstProxyServer;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        #location / {
           #root   html;
            #index  index.html index.htm;
        #}
		location ^~ /api {
            proxy_pass http://localhost:8081;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

	 server {
        listen       8092;
        server_name  secondProxyServer;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        #location / {
           #root   html;
            #index  index.html index.htm;
        #}
		location ^~ /api {
            proxy_pass http://localhost:8091;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
```


对于具体的 proxy_pass需要注意几个细节，具体如下


```
四种分别用http://127.0.0.1/SubProjectDir/default.html 进行访问。
 第一种：
location /SubProjectDir/ {
     proxy_pass http://127.0.0.1:81/;
}
会被代理到http://127.0.0.1:81/default.html 这个url

第二咱(相对于第一种，最后少一个 /)
location /SubProjectDir/ {
     proxy_pass http://127.0.0.1:81;
}
会被代理到http://127.0.0.1:81/SubProjectDir/default.html 这个url

第三种：
location /SubProjectDir/ {
     proxy_pass http://127.0.0.1:81/ftlynx/;
}
会被代理到http://127.0.0.1:81/ftlynx/default.html 这个url。

第四种情况(相对于第三种，最后少一个 / )：
location /SubProjectDir/ {
     proxy_pass http://127.0.0.1:81/ftlynx;
}
会被代理到http://127.0.0.1:81/ftlynxdefault.html 这个url
```

除此之外，为了解决跨域问题，需要设置http头以及一些相关配置。

```
 location / {
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Methods 'GET,POST,OPTIONS';
        add_header Access-Control-Allow-Credentials: true;
        add_header Access-Control-Allow-Headers 'Access-Control-Allow-Headers, Origin,Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers';
        proxy_pass_header Server;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
    }
```
