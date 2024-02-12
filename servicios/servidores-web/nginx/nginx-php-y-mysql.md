---
description: Una aplicación con PHP y MYSQL
---

# Nginx: PHP y MySQL

## Introducción

En esta sección vamos a ver el mismo ejemplo de aplicación web utilizando php y mysql que está configurado con docker, solo que, en este caso, vamos a ver cómo se haría la configuración nativa, en el sistema.

El ejemplo en cuestión es el mismo utilizado en la sección <mark style="color:blue;">`contenedores - docker - docker-compose - ejemplo`</mark>  para levantar un sitio web programado con <mark style="color:blue;">`PHP`</mark>, que se conecta a una `DB` en <mark style="color:blue;">`MySQL`</mark><mark style="color:blue;">,</mark> utilizando la actividad llamada <mark style="color:blue;">`LoginRegister`</mark> realizada en 1º año de Asix. Para ello necesitamos los servicios activos de:

* Apache o <mark style="color:blue;">`Nginx`</mark> para levantar el servicio web
* <mark style="color:blue;">`MySql`</mark> para la base de datos
* <mark style="color:blue;">`PHP`</mark> para el lenguaje de programación
* <mark style="color:blue;">`Phpmyadmin`</mark>, si me quiero conectar a un gestor de DB, aunque no lo voy a utilizar.

### Configurando la aplicación web

En el directorio <mark style="color:blue;">`/var/www/`</mark> creamos una carpeta que va a contener nuestros archivos de la aplicación web. Yo le he llamado simplemente <mark style="color:blue;">`example.com`</mark>:

<figure><img src="../../../.gitbook/assets/image (7) (6).png" alt=""><figcaption><p>/var/www/example.com</p></figcaption></figure>

Como podréis observar el dueño de la carpeta se llama www-data y no root, es es porque tenemos que modificar los archivos del directorio:

```
chown -R www-data:www-data /var/www/example.com
```

Volcamos el contenido dentro del directorio obteniendo lo siguiente:

<figure><img src="../../../.gitbook/assets/image (37).png" alt=""><figcaption><p>Contenido en /var/www/example.com</p></figcaption></figure>

Tendrás que tener en cuenta que en el archivo <mark style="color:blue;">`conexion.php`</mark> debes modificar los parámetros de conexión a la base de datos, como mínimo el password:

<figure><img src="../../../.gitbook/assets/image (11) (4).png" alt=""><figcaption><p>Archivo conexion.php</p></figcaption></figure>

### Configurando Nginx&#x20;

Vamos ahora a la configuración de Nginx. Para ello nos vamos al directorio <mark style="color:blue;">`/etc/nginx/sites-available/`</mark> creamos (podemos copiar y pegar del archivo <mark style="color:blue;">`default`</mark>) un archivo con el nombre de nuestro sitio web. En este caso, <mark style="color:blue;">`example.com.conf`</mark>:

```
server { 
  listen 80; 
  ## define root path
  root /var/www/example.com;

  error_log  /var/log/nginx/error-example.log;
  access_log /var/log/nginx/access-example.log;
  index index.php index.html;

  ## define location php
  location ~ \.php$ {
           include snippets/fastcgi-php.conf;
           fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
  }
}
```

Recuerda que tenemos que crear un enlace directo en <mark style="color:blue;">`/etc/nginx/sites-enable/example.com.conf`</mark>&#x20;

```
$ sudo ln -s /etc/nginx/sites-available/example.com.conf /etc/nginx/sites-enabled/
```

Ahora solo nos queda comprobar que no hayamos cometido un error con la configuración y actualizar el servicio de Nginx:

```
sudo nginx -t
sudo nginx -s reload
```



Para poder acceder a nuestro sitio web con el nombre de dominio que le hemos asignado tenemos que modificar el archivo /etc/hosts:

<figure><img src="../../../.gitbook/assets/image (34).png" alt=""><figcaption><p>Archivo /etc/hosts</p></figcaption></figure>

Finalmente, nos vamos al navegador y accedemos a nuestro sitio:

<figure><img src="../../../.gitbook/assets/image (17) (1).png" alt=""><figcaption><p>example.com</p></figcaption></figure>

