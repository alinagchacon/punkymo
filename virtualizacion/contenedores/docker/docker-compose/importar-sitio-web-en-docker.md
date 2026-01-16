# Importar sitio web en Docker

El directorio de trabajo será el siguiente:

<figure><img src="../../../../.gitbook/assets/image (444).png" alt="" width="272"><figcaption></figcaption></figure>

El archivo de configuración de nginx: default.conf. Tened en cuenta la línea: fastcgi\_pass app\_php:9000;

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
        fastcgi_pass app_php:9000;
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

El **dockerfile** que construye MySQL:&#x20;

```
FROM php:8.2-fpm
RUN docker-php-ext-install mysqli pdo pdo_mysql
```

Nuestro archivo de configuración: **docker-compose.yml**

```
services:
  # PHP service
  app:
    build: .
    container_name: app_php
    ports:
      - "9000:9000"
    volumes:
      - ./web:/var/www/lr/
      - ./log/php:/var/log/fpm-php.www.log
    working_dir: /var/www/lr
    networks:
      - netapp

  # MySQL database service
  appdb:
    image: mysql:8
    container_name: appdb
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: 1234
    volumes:
      #- ./mysql/:/var/lib/mysql
      - ./DB/:/DB/
    networks:
      - netapp

# PHPMYADMIN
  phpmyadmin:
    image: phpmyadmin
    container_name: appmyadmin
    environment:
      PMA_ARBITRARY: 1
    ports:
      - 8080:80
    networks:
      - netapp

  # Nginx service
  nginx:
    image: nginx
    container_name: appnginx
    ports:
      - 88:80
    volumes:
      - ./web:/var/www/lr/
      - ./nginx:/etc/nginx/conf.d/
      - ./log/nginx:/var/log/nginx/
    networks:
      - netapp

networks:
  netapp:
    driver: bridge
```

```
$username="root";
$password="1234";
$database="users"; 
```



