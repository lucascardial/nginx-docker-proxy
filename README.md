# NginxProxy #

Set your nginx-proxy service container to accepts any docker container domains

### Step 1 ###

Clone this repository an 'docker-common/nginx-proxy' folder (preferably)

### Step 2 ###

run docker-compose up -d in clonned folder

### Step 3 ###

Define yours VIRTUAL_HOST on docker-compose.yml of your projects, link this:

```
services:
  mysql:
    image: mysql:5.7
    container_name: mysql
    volumes:
      - ./dockerDB/:/var/lib/mysql
    ports:
      - "3307:3306"
    environment:
      - MYSQL_ROOT_PASSWORD=secret
      - MYSQL_DATABASE=dbname
      - MYSQL_USER=user
      - MYSQL_PASSWORD=secret
  
  laravel:
    depends_on:
      - mysql
    image: ambientum/php:7.2-nginx
    container_name: laravel
    volumes:
      - .:/var/www/app
    ports:
      - 8080
    restart: always
    environment:
      VIRTUAL_HOST: your-prefer-domain.test
networks:
  default:
    external:
      name: nginx-proxy
    
```

### Step 4 ###

Add your VIRTUAL_HOST to host file:

````
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
127.0.0.1       your-prefer-domain.test
````

### See an complete tutorial: ###
[https://blog.ssdnodes.com/blog/tutorial-using-docker-and-nginx-to-host-multiple-websites/](https://blog.ssdnodes.com/blog/tutorial-using-docker-and-nginx-to-host-multiple-websites/)