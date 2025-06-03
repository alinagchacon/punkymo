# Importar sitio web en Docker

Directorio de trabajo

<figure><img src="../../../../.gitbook/assets/image (3) (1).png" alt="" width="342"><figcaption></figcaption></figure>

default.conf

```
server {
    listen 80;
    index index.php index.html;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;

    ## define root path
    root /var/www/punky;

    ## define location php
    location ~ \.php$ {
        try_files $uri =404;

        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass phpfpm:9000;
        fastcgi_index index.php;
        include fastcgi_params;

        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }

    location / {
        try_files $uri $uri/ /index.php?$query_string;
        gzip_static on;
    }
}

```

docker-compose.yml

```
 archivo docker-compose.yml

services:
  # PHP service
  phpfpm:
    image: php:8-fpm-alpine
    container_name: phpfpm
    working_dir: /var/www/punky
    ports:
      - "9000:9000"
    volumes:
      - './web:/var/www/punky'
      #- './phpfpm/php-fpm.d/www.conf:/usr/local/etc/php-fpm.d/www.conf'
    restart: always
    networks:
      - netweb

  # Nginx service
  nginx:
    image: nginx:alpine
    container_name: nginx
    ports:
      - 8082:80
    working_dir: /etc/nginx
    volumes:
      - './web:/var/www/punky'
      - './nginx/default.conf:/etc/nginx/conf.d/default.conf'
      - './nginx/:/var/log/nginx/'
    restart: always
    networks:
    - netweb


  # MySQL database service
  db:
    image: mysql
    container_name: miDB
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: 1234
    volumes:
      - './mysql:/var/lib/mysql'
      - './db:/db'
    networks:
      - netweb

  # PHPMYADMIN
  phpmyadmin:
    image: phpmyadmin
    container_name: miphpmyadmin
    environment:
      PMA_ARBITRARY: 1
    ports:
      - 81:80
    networks:
      - netweb
      
networks:
  netweb:
     driver: bridge
```

En conexión.php

```
$servername="db";
$username="root";
$password="1234";
$database="users"; 
```



