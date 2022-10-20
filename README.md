### Introduction 
This project base on laradock
[laradock](https://github.com/laradock/laradock) 

On my job,I must develpment mutli version website.<br>
So I use laradock work for me ,and remove something don't need ,and import my idea in there.

This progject resolved:
 - multi website on one server connection problem
 - multi container port setting 
 - DNS resolve 
 - switch multi php verison 
 - mysql version
 - timezone setting for all servise
 
### incloud service(master)
- DB:
    - mysql5.7
    - mysql8
    - redis
- Web Server:
    - nginx
- php-fpm:
    - php5.6
    - php7.4
    - php8.1
- workspace
    - npm + node.js 
    - composer + php7.4    
- workspace2
    - npm + node.js 
    - composer + php8.1  
### Server Requirements
- docker
    - version: 20.10.11
- docker compose 
    - version: '3.5'
### local service port(not in container)
- DB:
    - mysql5.7:3306
    - mysql8:3307
    - redis:6379
### notice
Folder(docker folder and webiste on same level)
 - docker_multi_server
    - nginx
    - php-fpm8.1
    - mysql8
 - website1_project
 - website2_project

Local DNS setting on 3 where
1.nginx/sites >>add config file
<br>example
``` config
server {
    listen 80;
    #listen [::]:80 default_server ipv6only=on;
    server_name admin.web1.test;
    root /var/www/web1/;
    index index.php index.html index.htm;
    location / {
         try_files $uri $uri/ /index.php$is_args$args;
    }
    location ~ \.php$ {
        try_files $uri /index.php =404;
        #fastcgi_pass php-upstream;
        #choose your php-fpm
        #nginx and  php-fpm in conatiner swith data so use 9000 port
        fastcgi_pass php-fpm7.4:9000;
        fastcgi_index index.php;
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        #fixes timeouts
        fastcgi_read_timeout 600;
        include fastcgi_params;
    }
    location ~ /\.ht {
        deny all;
    }

    location /.well-known/acme-challenge/ {
        root /var/www/letsencrypt/;
        log_not_found off;
    }
}

```
2.docker-compose.yml >> NGINX Server aliases
```
nginx:
      build:
        context: ./nginx
      networks:
        frontend:
          aliases: 
           - "admin.web1.test;" 
           - "admin.web2.test;" 
        backend:
          aliases: 
           - "admin.web1.test;" 
           - "admin.web2.test;" 
```
3.if your development on Windows ,don't forget edit local host file.
```
C:\Windows\System32\drivers\etc
```     
### Common Command
```bash
#build image
docker compose build
```
```bash
#create container & run ccontainer
docker compose up
```
```bash
#remove container
docker compose down
```
### develpment on workspace
```bash
#php 7.4
docker compose exec workspace bash
```
```bash
#php 8.1
docker compose exec workspace2 bash
```


    