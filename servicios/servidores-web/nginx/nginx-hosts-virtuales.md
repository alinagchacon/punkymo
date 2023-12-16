# NGINX, hosts virtuales

Nginx está compuesto por una serie de módulos que son controlados por directivas especificadas en el archivo de configuración.

Estas directivas se dividen en simples y de bloque.

* Una <mark style="color:blue;">`directiva simple`</mark> consiste en el nombre y los parámetros separados por espacio y terminados en punto y coma <mark style="color:blue;">`;`</mark>
* Una <mark style="color:blue;">`directiva de block`</mark> tiene la misma estructura que una simple, pero en lugar de <mark style="color:blue;">`;`</mark> termina con un conjunto adicional de instrucciones entre llaves <mark style="color:blue;">`{ }`</mark>.  Si una directiva de bloque contiene otras directivas dentro de llaves, se denomina contexto.

Se considera que están dentro del contexto principal aquellas directivas colocadas en el archivo de configuración fuera de cualquier contexto.

Los eventos y las directivas http residen en el contexto principal, el servidor en http y la ubicación en el servidor.

```
http {
      server {
      }
}
```

```
location / {
      root /var/www;       
}
```

### &#x20;Archivos de configuración

Los archivos de configuración de Nginx se encuentran en el directorio:&#x20;

```
/etc/nginx
```

y el archivo de configuración global es:&#x20;

```
/etc/nginx/nginx.conf
```

La estructura de configuración del archivo de configuración principal se muestra como sigue:

<figure><img src="../../../.gitbook/assets/image (11) (2).png" alt=""><figcaption><p>Archivo de configuración /etc/nginx/nginx.conf</p></figcaption></figure>

Tener en cuenta que, en Ubuntu existe una directiva de inclusión adicional:

```
include/etc/nginx/sites-enabled/*;
```

porque el directorio <mark style="color:blue;">`/etc/nginx/sites-enabled/`</mark> almacena los enlaces simbólicos a los archivos de configuración de sitios web creados en <mark style="color:blue;">`/etc/nginx/sites-available/`</mark> para habilitar los sitios. Y el hecho de eliminar un enlace simbólico deshabilita ese sitio en particular.

El directorio donde se localizan todos los archivos de configuración de Nginx es /etc/nginx/ y dentro debemos tener en consideración <mark style="color:blue;">`/etc/nginx/sites-available, /etc/nginx/sites-enable`</mark> y el propio <mark style="color:blue;">`/etc/nginx/nginx.conf`</mark> que vimos anteriormente.

<figure><img src="../../../.gitbook/assets/image (1) (1) (2).png" alt=""><figcaption><p>Directorio /etc/nginx</p></figcaption></figure>

#### Archivo /sites-available/default

Este sería el bloque del servidor Nginx predeterminado de muestra ubicado en

<mark style="color:blue;">`/etc/nginx/conf.d/default.conf`</mark> en el sistema de prueba.

&#x20;

<figure><img src="../../../.gitbook/assets/image (8) (4) (1).png" alt=""><figcaption><p>Fichero /etc/nginx/sites-available/default</p></figcaption></figure>

Donde:

* **listen**: especifica el puerto en el que escucha el servidor.
* **server\_name**: define el nombre del servidor, que puede ser nombres exactos, nombres comodín o expresiones regulares.
* **root**: especifica el directorio desde el cual Nginx servirá páginas web y otros documentos.
* **index**: especifica los tipos de archivos de índice que se publicarán.
* **location**: se utiliza para procesar solicitudes de archivos y carpetas específicos.

Solo de tener instalado el Nginx, si escribimos: localhost en el navegador veremos su página por defecto. Esto es:

<figure><img src="../../../.gitbook/assets/image (20) (2) (1).png" alt=""><figcaption><p>Página web por defecto de Nginx</p></figcaption></figure>

### &#x20;Sirviendo contenido estático

Es de suponer que una tarea importante del servidor web es entregar archivos del tipo imágenes o páginas HTML estáticas. Vamos a implementar un ejemplo en el que, según la solicitud, los archivos se entregarán desde diferentes directorios locales <mark style="color:blue;">`/var/www/`</mark> con archivos HTML y <mark style="color:blue;">`/var/www/images`</mark> que contiene imágenes.

Hay que tener en cuenta que debemos crear nuestro contenido web en un directorio. Podemos utilizar el directorio por defecto: <mark style="color:blue;">`/var/www/`</mark>  y si lo analizamos, veremos que dentro se encuentra una carpeta llamada <mark style="color:blue;">`/var/www/html`</mark> que contiene el archivo que se muestra desde el navegador: <mark style="color:blue;">`index.nginx-debian.html`</mark> cuando escribimos: <mark style="color:blue;">`localhost`</mark>  en la barra de navegación.&#x20;

#### Sitios web de prueba

En mi caso, crearé dos directorios dentro de la carpeta <mark style="color:blue;">`/var/www/`</mark>:

<mark style="color:blue;">`/var/www/example1.com`</mark> y <mark style="color:blue;">`/var/www/example2.com`</mark>

Dentro de <mark style="color:blue;">`/var/www/example1.com`</mark> creamos una carpeta para las imágenes. En este caso:

&#x20;<mark style="color:blue;">`/var/www/example1.com/img`</mark> de modo que tendremos una estructura como la siguiente:

<figure><img src="../../../.gitbook/assets/image (9) (1) (3).png" alt=""><figcaption><p>Contenido del sitio example1.com</p></figcaption></figure>

El archivo <mark style="color:blue;">`index.html`</mark> es muy simple:

<pre><code>&#x3C;!DOCTYPE html>
&#x3C;html>
&#x3C;head>
&#x3C;title>Punky Web&#x3C;/title>
&#x3C;/head>
&#x3C;body>
 
&#x3C;h1>Remember Coffee&#x3C;/h1>
&#x3C;p>My sweet owl&#x3C;/p>
&#x3C;img src="img/cafe.jpg" />
<strong>Sin café no funciono!!!
</strong>&#x3C;/body>
&#x3C;/html>
</code></pre>

Para el caso de <mark style="color:blue;">`example2.com`</mark> hacemos algo similar.&#x20;

```
<!DOCTYPE html>
<html>
<head>
<title>Alita Web</title>
</head>
<body>
 
<h1>Papillon</h1>
<p>Siempre</p>
<img src="img/papillon.jpg" />
</body>
</html>
```

#### Archivos de configuración en nginx

Ya tenemos dos sitios web. Ahora tenemos que crear los ficheros de configuración dentro de <mark style="color:blue;">`/etc/nginx/sites-available`</mark>:

```
/etc/nginx/sites-available/example1.conf
/etc/nginx/sites-available/example2.conf
```

En el archivo de configuración predeterminado <mark style="color:blue;">`default`</mark> ya se incluye un ejemplo del bloque del servidor. Se encuentra comentado pero lo podemos utilizar copiando y pegando el propio archivo default, des comentamos las líneas y adecuamos a nuestras necesidades.

Por tanto, el archivo de configuración para el sitio web <mark style="color:blue;">`example1.com`</mark> sería <mark style="color:blue;">`/etc/nginx/sites-available/example1.conf`</mark>&#x20;

<pre><code><strong>server {
</strong>        listen 8080;
        root /var/www/example1.com;
        index index.html;
        server_name example1.com www.example1.com;

        location / {
                try_files $uri $uri/ =404;
        }

        location /images/{
                root /img;
        }
}
</code></pre>

Y similar para el caso del sitio web <mark style="color:blue;">`example2.com`</mark> sería <mark style="color:blue;">`/etc/nginx/sites-available/example2.conf`</mark>&#x20;

```
server {
        listen 8081;
        root /var/www/example2.com;
        index index.html;
        server_name example2.com www.example2.com;

        location / {
                try_files $uri $uri/ =404;
        }

        location /images/{
                root /img;
        }
}
```

Tener en cuenta que hemos utilizado un puerto diferente para cada sitio web: el 8080 para el example1.com y el 8081 para el example2.com.

### Resultado

Podremos visualizar las dos páginas web desde un equipo cliente (en mi caso Ubuntu Desktop por red interna) utilizando la IP aunque no el dominio, esto es:

[http://192.168.6.100:8080](http://192.168.6.100:8080) (café)

[http://192.168.6.100:8081](http://192.168.6.100:8081) (papillon)

<figure><img src="../../../.gitbook/assets/image (25) (2).png" alt=""><figcaption><p>Sitio web www.example1.com </p></figcaption></figure>

Si quiero verificar los puertos abiertos en el servidor podemos hacer:

sudo netstat –tlpn | grep nginx

y nos muestra lo siguiente:

<figure><img src="../../../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p>Los puertos habilitados para Nginx</p></figcaption></figure>

## Acceso personalizado

En lugar de utilizar los archivos de registro predeterminados, podemos definir archivos de registro personalizados para diferentes sitios web.

Para restringir el acceso a un sitio web/aplicación o algunas partes del mismo, podemos configurar la autenticación HTTP **básica**. Esto se puede utilizar esencialmente para restringir el acceso a todo el servidor HTTP, bloques de servidores individuales o bloques de ubicación. Para ello instalamos <mark style="color:blue;">`apache2-utils`</mark>:

```
sudo apt install apache2-utils
```

A continuación, mediante la utilidad <mark style="color:blue;">`htpasswd`</mark>, crearemos el archivo que almacenará nuestras credenciales de acceso (nombre de usuario/contraseña). Y como ejemplo, agreguemos el administrador de usuarios (en mi caso **kirby**) a esta lista. Tened en consideración que podemos agregar tantos usuarios como necesitemos.&#x20;

Una vez que presione Enter, se te pedirá ingresar la contraseña del usuario:

```
$ sudo htpasswd -Bc /etc/nginx/conf.d/.htpasswd kirby
```

Las opciones que vamos a utilizar son:

\-c  especificar el archivo de contraseña\
\-B  cifrar la contraseña.&#x20;

&#x20;Para mayor información acerca del funcionamiento de la herramienta <mark style="color:blue;">`htpasswd`</mark>, puedes hacer:

```
man htpasswd
```

&#x20;

<figure><img src="../../../.gitbook/assets/image (21) (2).png" alt=""><figcaption><p>Archivo .htpasswd</p></figcaption></figure>

Luego, asignamos los permisos y la propiedad adecuados al archivo de contraseña (reemplaza el usuario y grupo por <mark style="color:blue;">`www-data`</mark>).

```
$ sudo chmod 640 /etc/nginx/conf.d/.htpasswd
```

```
$ sudo chown nginx:nginx /etc/nginx/conf.d/.htpasswd
```

Se puede restringir el acceso al servidor web, un solo sitio web (usando su bloque de servidor) o un directorio o archivo específico. Se pueden usar dos directivas útiles para lograr esto:

* **auth\_basic**: activa la validación del nombre de usuario y la contraseña mediante el protocolo "Autenticación básica HTTP".
* **auth\_basic\_user\_file**: especifica el archivo de la credencial.

### Protegiendo un directorio específico

Veamos a ver cómo proteger con contraseña el directorio <mark style="color:blue;">`/var/www/kirby.com/protected`</mark>. Dentro de la carpeta /protected he puesto el archivo <mark style="color:blue;">`index.html`</mark> para hacer la prueba. &#x20;

Para ello agregamos el bloque <mark style="color:blue;">`location /protected`</mark> en el archivo de configuración <mark style="color:blue;">`kirby.com.conf:`</mark>

```
server {
    listen         80;
    server_name    www.kirby.com kirby.com;
    root           /var/www/kirby.com/;
    index          index.php  index.html;
    location / {
                try_files $uri $uri/ =404;
        }
    
    location /protected/ {
        auth_basic              "Restricted Access!";
        auth_basic_user_file    /etc/nginx/conf.d/.htpasswd;
    }
    
    location ~ /.php {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm:sock;
    }
    
}
```

Ahora, guardamos los cambios y reiniciamos el servicio Nginx.

```
$ sudo systemctl restart nginx
```

o bien:

```
$ sudo nginx -s reload
```

Con esto, la próxima vez que intentes acceder a ese directorio <mark style="color:blue;">`http:// localhost/protected`</mark> te pedirá que ingreses las credenciales de inicio de sesión (en mi caso user: <mark style="color:blue;">`kirby`</mark> y password: la elegida).

Un inicio de sesión exitoso te permite acceder al contenido del directorio; de lo contrario, obtendrá un error de <mark style="color:blue;">`Se requiere autorización 401`</mark>.

<figure><img src="../../../.gitbook/assets/image (4) (1) (2).png" alt=""><figcaption><p>Drectorio /var/www/html/protected</p></figcaption></figure>

&#x20;En este ejemplo no hemos creado ningún contenido dentro del directorio <mark style="color:blue;">`protected`</mark> así que para testear correctamente el sitio deberíamos hacerlo.

