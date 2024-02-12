---
description: docker-compose
---

# Docker: PHP y MySQL

Utilizaremos a modo de ejemplo la creación de varios contenedores relacionados entre sí, para levantar un sitio web programado con PHP, que se conecta a una DB en MySQL. De hecho, voy a utilizar una actividad llamada `LoginRegister` realizada en 1º año de Asix. Para ello necesitamos los servicios activos de:

* Apache o <mark style="color:blue;">`Nginx`</mark> para levantar el servicio web
* <mark style="color:blue;">`MySql`</mark> para la base de datos
* <mark style="color:blue;">`PHP`</mark> para el lenguaje de programación
* <mark style="color:blue;">`Phpmyadmin`</mark>, si me quiero conectar a un gestor de DB.

Veamos cómo levantar un <mark style="color:blue;">`docker-compose`</mark> creando un archivo de configuración <mark style="color:blue;">`docker-compose.yml`</mark>, dado que el <mark style="color:blue;">`docker-compose`</mark> busca, por defecto,  las instrucciones en el archivo <mark style="color:blue;">`docker-compose.yml`</mark>.

Adicionalmente, tengamos en cuenta que docker lo tengo instalado en una VM con `Ubuntu Desktop 22.04 LTS` con `Portainer` y `Docker-compose`.

### Directorios - volúmenes

Como vamos a crear los volúmenes de Docker para tener acceso directo a los contenidos en local, tenemos que crear los directorios y los contenidos necesarios.  Los archivos importantes a tener en cuenta serían los ficheros de configuración de nginx y los .php de la aplicación web. Para ello, he creado un directorio con los contenidos necesarios:

<table data-header-hidden><thead><tr><th width="208.5">Directorio</th><th>Contenido</th></tr></thead><tbody><tr><td>Directorio</td><td>Contenido</td></tr><tr><td>LoginRegister</td><td>La aplicación web con sus archivos .php, .css, etc.</td></tr><tr><td>DB</td><td>La DB <code>users1</code> que utiliza la aplicación web. Realmente no es necesario, lo hago por comodidad.</td></tr><tr><td>log</td><td>Para almacenar los log de nginx (error, access) y de php.</td></tr><tr><td>Nginx/conf.d</td><td>En este directorio guardamos el archivo de configuración: <code>default.conf</code></td></tr></tbody></table>

&#x20;La actividad <mark style="color:blue;">`LoginRegister`</mark> tiene una estructura de archivos como se muestra en la imagen siguiente. Tener en cuenta que el directorio <mark style="color:blue;">`mysql`</mark> no es necesario.

<figure><img src="../../../../.gitbook/assets/image (13) (3).png" alt=""><figcaption><p>Estructura de archivos .php de la aplicación web</p></figcaption></figure>

### Archivos de configuración

Tenemos que preparar dos archivos de configuración:

* default.conf
* docker-compose.yml

#### Default.conf

En este archivo tenemos las siguientes directivas:

**Listen** -  define qué dirección IP y qué puertos escucha el servicio. En este caso, Nginx escucha en el puerto 80 en todas las direcciones IPv4 e IPv6. Si establecemos el parámetro <mark style="color:blue;">`default_server`</mark> le indicamos a Nginx que utilice este bloque server por defecto para las peticiones que coincidan con las direcciones IP y los puertos.

**server\_name** - define los nombres de host de los que es responsable este bloque server. Establecer <mark style="color:blue;">`server_name`</mark> a \_ permite configurar Nginx para aceptar cualquier nombre de host para este bloque server.

**root** - establece la ruta del contenido web para este bloque server.

**log** - También hemos configurado los archivos para los log files, que utilizaremos también dentro de los volúmenes de docker.

Aquí dejo como ejemplo el archivo que he utilizado:

```
server {
    listen 80;
    index index.php index.html;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    
    ## define root path
    root /var/www/html;
    
    ## define location php
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass app:9000;
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

#### **Docker-compose.yml**&#x20;

Del mismo modo que existen los <mark style="color:blue;">`Dockerfile`</mark>, donde se puede configurar el estado de un contenedor de manera declarativa, en docker-compose existe el equivalente: los archivos `.yml`.

Por tanto, un archivo de docker-compose es un archivo con extensión y formato yml. Para usarlo basta con crearlo y empezar a agregar el contenido.

El archivo tiene una estructura bastante fácil de entender. Comienza por especificar la versión de docker compose que se utilizará:

<figure><img src="../../../../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption><p>Estructura de un archivo docker-compose.yml</p></figcaption></figure>

Después de la versión viene  la sección de servicios. Puede haber tantos servicios como queramos: servidor web, base de datos, documentación, etc. Cada servicio cuenta con sus propias variables de configuración y sus respectivos valores.

**version 3.8**: Es muy importante indicar la versión de las instrucciones que vamos a utilizar. Docker evoluciona, pero siempre hay compatibilidad con las versiones anteriores.

**app**: Indica el nombre del servicio, que podría ser cualquiera. El nombre indica el tipo de servicio que estamos construyendo. Por ejemplo: MySQL, Nginx, etc.

**Nombres de servicios:** El nombre que usamos para cada servicio dentro de nuestro archivo <mark style="color:blue;">`yml`</mark> nos sirve como referencia para su uso en otros servicios.

Por ejemplo, si llamamos a un servicio como <mark style="color:blue;">`db`</mark>, entonces es <mark style="color:blue;">`db`</mark> el nombre que debemos usar en otras aplicaciones para referirnos a un host o ubicación, incluso una base de datos. Veamos algunas de las variables de configuración (que se pueden consultar en documentación oficial de Docker).

**image:** establece la imagen a partir de la cual se generará el servicio, ideal cuando el servicio no necesita de una personalización muy complicada.

**build:**  si necesitamos una imagen personalizada pudiera ser mejor usar un Dockerfile. La opción build nos permite indicar el directorio donde se encuentra. Indica dónde está el Dockerfile a utilizar para crear el contenedor. Si escribimos <mark style="color:blue;">`build .`</mark> se considera que el Dockerfile está en el directorio actual.&#x20;

**command:** sobre escribe el comando predeterminado del contenedor. Esta opción es ideal para ejecutar un comando cuando inicia un servicio, por ejemplo, un servidor web.

**ports:** nos dice los puertos externos e internos que se vincularán, siempre en el formato de <mark style="color:blue;">`host:contenedor`</mark>. También podemos especificar los protocolos <mark style="color:blue;">`udp`</mark> o <mark style="color:blue;">`tcp`</mark>.

**expose:** también expone puertos. La diferencia con el comando <mark style="color:blue;">`ports`</mark> es que los puertos solo estarán disponibles para los servicios vinculados, no para la máquina desde donde estamos ejecutando <mark style="color:blue;">`docker-compose`</mark>.

**depends\_on:** Si queremos que uno de nuestros servicios se ejecute después de otro. Por ejemplo, para que un servidor web funcione correctamente es necesario tener una base de datos que ya se encuentre en funcionamiento.

**environment:** Permite establecer una lista de [`variables de entorno`](https://coffeebytes.dev/comandos-de-linux-que-deberias-conocer-tercera-parte/) que estarán disponibles en nuestro servicio.

<details>

<summary>Por si no recuerdas lo que son las variables de entorno</summary>

Tanto Windows, Linux como Mac utilizan ciertos valores para almacenar información que pueden variar de un equipo a otro o de un usuario a otro. Son valores que hacen referencia a archivos, directorios y funciones comunes del sistema cuya ruta concreta puede variar, pero que otros programas necesitan poder conocer.

&#x20;Un ejemplo de este tipo de variable puede ser:

* la ubicación de un archivo en el sistema,
* una lista de objetos, número de versión

&#x20;Algunas de las variables de entorno más comunes son:

**PATH**: es la lista de directorios donde la Shell o intérprete busca los comandos.

**PWD**: nos muestra el directorio actual.

**BASH\_VERSION**: versión de [Bash](https://keepcoding.io/devops/que-es-bash-shell-y-como-funciona) que estemos utilizando.

**RANDOM**: tiene la función de generar un número aleatorio.

**HOSTNAME**: nombre del sistema.

**MACHTYPE**: arquitectura del sistema.

**HISTFILE**: fichero donde se guarda la historia de comandos.

**HOME**: directorio home del usuario.

</details>

**volumes:** Se puede enviar partes de nuestro S.O a un servicio usando uno o varios volúmenes. Para esto usamos la sintaxis <mark style="color:blue;">`host:contenedor`</mark>, donde <mark style="color:blue;">`Host`</mark> puede ser una ubicación en tu sistema o también el nombre de un volumen que hayas creado con docker. Hace un mapeo entre el directorio del host y el directorio del contenedor. De este modo, cualquier cambio en el directorio local en el host, se hará de inmediato en el contenedor.

**restart :** Podemos aplicar políticas de reinicio a nuestros servicios. Puede tomar varios valores:

* **no**: nunca reinicia el contenedor
* **always**: siempre lo reinicia
* **on-failure**: lo reinicia si el contenedor devuelve un estado de error
* **unless-stopped**: lo reinicia en todos los casos excepto cuando se detiene

A continuación, un ejemplo del archivo <mark style="color:blue;">`docker-compose.yml`</mark> que he utilizado:

```
# archivo docker-compose.yml

version: "3.8"
services:

  # PHP service
  app:
    image: php:8-fpm
    container_name: miAppPHP    
    volumes:
      - LoginRegister:/var/www/loginregister/
      - LoginRegister/log/php.log:/var/log/fpm-php.www.log
    networks:
      - app-network
  # MySQL database service
  db:
    image: mysql:8.0
    container_name: miAppMySQL
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: 1234
    volumes:
      - LoginRegister/mysql/:/var/lib/mysql
      - LoginRegister/DB/:/DB/
    networks:
      - app-network

  # PHPMYADMIN
  phpmyadmin:
    image: phpmyadmin
    container_name: miAppPhpMyAdmin
    working_dir: /
    environment:
      PMA_ARBITRARY: 1      
    ports:
      - 8080:80
    networks:
      - app-network

  # Nginx service
  nginx:
    image: nginx
    container_name: miAppNginx
    ports:
      - 88:80
    volumes:
      - LoginRegister:/var/www/loginregister/
      - LoginRegister/nginx/conf.d:/etc/nginx/conf.d/
      - LoginRegister/log/nginx:/var/log/nginx/
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

```

Como podemos ver tenemos que utilizar la imagen de PHP, <mark style="color:blue;">`FPM - FastCGI Process Manager`</mark> que es una implementación alternativa al PHP FastCGI con algunas características adicionales (la mayoría) útiles para sitios web con mucho tráfico.

&#x20;Un par de características que posee:

* Manejo avanzado para detener/arrancar procesos de forma fácil.
* Posibilidad de iniciar hilos de procesos con diferentes uid/gid/chroot/environment, escuchar en diferentes puertos y usar distintos php.ini (remplazando).

<details>

<summary>Dentro de MySQL</summary>

En caso de tener que instalar directamente la DB podemos hacer lo siguiente, dentro del propio contenedor:

Acceder al mysql para crear y copiar la DB:

<pre><code><strong>mysql –p –u root
</strong></code></pre>

Crear la DB:

```
mysql> create database users1;
```

Copiar la DB users.sql en la DB creada users1:\


```
mysql -p -u root --password=1234 users1 < users.sql 
```

&#x20; Algunos comandos básicos de MySQL:\


```
use Database users;
show tables;
describe users;
select * from users1.users;
```

</details>

### Creando el docker-compose desde Portainer

Ahora es que estamos en condiciones de crear nuestro docker-compose. Para ello nos vamos a Portainer.

<figure><img src="../../../../.gitbook/assets/image (25) (1).png" alt=""><figcaption><p>En Portainer</p></figcaption></figure>

Nos vamos a la herramienta stacks de Portainer, donde podremos crear nuestra "composición" de servicios. Recordemos que un stack es una colección de servicios que están relacionados con una aplicación, como por ejemplo,  WordPress que incluye un contenedor de servidor web (como nginx o apache) y un contenedor de base de datos como MySQL.

Una vez en el apartado vamos a crear un nuevo stack:

<figure><img src="../../../../.gitbook/assets/image (7) (6) (1).png" alt=""><figcaption><p>Creamos un nuevo stack</p></figcaption></figure>

Se nos abrirá una nueva página donde podemos aplicar diferentes opciones para crear una colección entre las que tenemos subir nuestro docker-compose.yml, usar un repositorio de git. Aquí utilizaremos el editor web:

<figure><img src="../../../../.gitbook/assets/image (1) (2) (2).png" alt=""><figcaption><p>Stack en Portainer</p></figcaption></figure>

En el editor web volcamos el contenido de nuestro archivo <mark style="color:blue;">`docker-compose.yml`</mark> previamente creado. En caso de tener algún aparecerá una notificación en rojo que nos impedirá continuar &#x20;

<figure><img src="../../../../.gitbook/assets/image (61).png" alt=""><figcaption><p>docker-compose.yml para crear el stack</p></figcaption></figure>

Una vez hecho esto, clicamos a <mark style="color:blue;">`deploy`</mark> para crear y desplegar todos los contenedores. Si todo está correcto, nos crea la colección y podremos acceder a la misma y veremos algo como lo siguiente:

<figure><img src="../../../../.gitbook/assets/image (39) (2).png" alt=""><figcaption><p>Despliegue de contenedores</p></figcaption></figure>

Podemos observar los diferentes contenedores involucrados en desplegar la aplicación web:

* miAppPHP - es el contenedor de php
* miAppPhpMyAdmin - el contenedor para el gestor de bases de datos
* miAppNginx - el servidor web&#x20;
* miAppMySQL - La base de datos

#### Accediendo a PhpMyAdmin

<figure><img src="../../../../.gitbook/assets/image (224).png" alt=""><figcaption><p>Accediendo a la base de datos en Phpmyadmin</p></figcaption></figure>

#### Accediendo a la web

<figure><img src="../../../../.gitbook/assets/image (225).png" alt=""><figcaption><p>El sitio web de prueba</p></figcaption></figure>

## Links

* [https://docs.docker.com/compose/compose-file/build/](https://docs.docker.com/compose/compose-file/build/)
* [https://www.docker.com/blog/how-to-use-the-apache-httpd-docker-official-image/](https://www.docker.com/blog/how-to-use-the-apache-httpd-docker-official-image/)
* [https://academy.leewayweb.com/como-usar-docker-en-proyectos-php/](https://academy.leewayweb.com/como-usar-docker-en-proyectos-php/)
* [https://www.kodetop.com/crea-tu-ambiente-de-desarrollo-php-mysql-con-docker/](https://www.kodetop.com/crea-tu-ambiente-de-desarrollo-php-mysql-con-docker/)
* [https://hub.docker.com/\_/php/tags](https://hub.docker.com/\_/php/tags)
* [https://access.redhat.com/documentation/es-es/red\_hat\_enterprise\_linux/8/html/deploying\_different\_types\_of\_servers/setting-apache-http-server\_deploying-different-types-of-servers](https://access.redhat.com/documentation/es-es/red\_hat\_enterprise\_linux/8/html/deploying\_different\_types\_of\_servers/setting-apache-http-server\_deploying-different-types-of-servers) \*\*\*
* [https://www.php.net/manual/es/install.fpm.php](https://www.php.net/manual/es/install.fpm.php)
* [https://docs.portainer.io/user/docker/stacks](https://docs.portainer.io/user/docker/stacks)
