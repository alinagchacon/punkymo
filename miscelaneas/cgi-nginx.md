---
description: y PHP
---

# CGI - NGINX

Unos chiquillos de ASIX2 están trabajando con Arduino en su proyecto y necesitan habilitar el uso de CGI shell script en un equipo que ejecuta Ubuntu Server, así que vamos a ayudarles.

Vamos a mostrar cómo habilitar la característica CGI y permitir que se ejecute un CGI en el servidor Nginx.

Dado que nos interesa que esté configurado PHP y MySQL vamos a dar un repaso a toda la instalación y configuración de PHP con PHP-FPM para un servidor HTTP de Nginx 1.18.

Para realizar este tutorial estoy utilizando:&#x20;

* una VM con Ubuntu Server 22.04 que tiene instalado y configurado el servicio DNS y DHCP&#x20;
* está conectada en red NAT a otra VM que hace de cliente con un Debian.

### Instalación de PHP

Procedemos a la instalación de los paquetes necesarios de PHP:

```
sudo apt install  php8.1-mbstring php8.1-zip php8.1-gd php8.1-json php8.1-curl
sudo apt install php8.1-fpm php8.1-cgi
```

Los paquetes a instalar son extensiones de PHP 8.1 que proporcionan funcionalidades adicionales para desarrollar aplicaciones web en PHP. Por ejemplo:

1. **php8.1-mbstring**: Proporciona funciones para manejar cadenas que contienen caracteres no ASCII. Útil para trabajar con texto en diferentes idiomas.
2. **php8.1-zip**: Para trabajar con archivos y directorios comprimidos en formato ZIP.
3. **php8.1-gd**: Se trata de una biblioteca gráfica de manipulación de imágenes en PHP. Esta extensión permite a PHP crear y manipular imágenes de forma dinámica, como la creación de miniaturas, la composición de imágenes y la manipulación de formatos de imagen.
4. **php8.1-json**: Brinda funciones para trabajar con JSON en PHP. Permite codificar y decodificar datos en formato JSON. Realmente útil para el intercambio de datos entre aplicaciones web y servicios web, así como para el almacenamiento de datos estructurados.
5. **php8.1-curl**: Para realizar solicitudes HTTP y otras operaciones relacionadas con la transferencia de datos a través de URL utilizando la biblioteca libcurl.
6. **php8.1-fpm**:  FastCGI Process Manager (FPM) es un gestor de procesos FastCGI para PHP que proporciona un entorno de ejecución de PHP más eficiente y escalable para servidores web. FPM gestiona múltiples instancias de intérpretes o procesos PHP que están listos para procesar solicitudes de forma rápida y eficiente. FPM suele ser utilizado con servidores web como Nginx o Apache utilizando el módulo de proxy FastCGI para redirigir las solicitudes PHP al pool de procesos de FPM.
7. **php8.1-cgi:** Se trata de una interfaz común que permite a los servidores web comunicarse con programas externos, incluidos los scripts PHP. `php8.1-cgi` es una implementación de PHP que funciona como un intérprete CGI. Cada vez que se recibe una solicitud PHP, el servidor web inicia un nuevo proceso php8.1-cgi para ejecutar el script PHP correspondiente. Aunque es fácil de configurar y usar, el rendimiento puede ser menor en comparación con FPM, ya que php8.1-cgi debe iniciarse y detenerse para cada solicitud.

Lo siguiente fue configurar NGINX para que pudiera servir páginas en PHP. Para ello tengamos en cuenta que en /var/www/punky.com es donde se hospedará nuestro sitio web.

<figure><img src="../.gitbook/assets/image (303).png" alt=""><figcaption><p>Configuración de Nginx para el sitio web www.punky.com</p></figcaption></figure>

Almacenaremos los logs del sitio en `/var/log/nginx/error_punky.log` y  `/var/log/nginx/access_punky.log`.

SI revisamos alguna de las opciones de la configuración vemos que tenemos una carpeta llamada snippets en `/etc/nginx/snippets/` que contiene dos archivos de configuración:

<figure><img src="../.gitbook/assets/image (304).png" alt=""><figcaption><p>/etc/nginx/snippets/</p></figcaption></figure>

Y nos aseguramos ahora que todo funciones correctamente. Para ello he creado un archivo `index.php` en `/var/www/punky.com/` como se muestra en la imagen:

<figure><img src="../.gitbook/assets/image (306).png" alt=""><figcaption><p>/var/www/punky.com/index.php</p></figcaption></figure>

Si nos vamos al navegador del equipo cliente (Debian) y llamamos a nuestra página web se nos mostrará lo siguiente:

<figure><img src="../.gitbook/assets/image (305).png" alt=""><figcaption><p>La página web de prueba en punky.com</p></figcaption></figure>

## La parte del CGI

Llegamos a este punto me instalé el paquete `fcgiwrap` que es un programa que se utiliza de interfaz entre los servidores web y las aplicaciones FastCGI. Digamos que permite a los servidores web la comunicación con aplicaciones externas de un modo más eficiente que la manera tradicional con CGI.&#x20;

Al usar FastCGI, las aplicaciones se pueden mantener en ejecución, reduciendo el tiempo de carga con lo que  se mejora el rendimiento general del servidor web.Por tanto, `fcgiwrap` actúa como un puente entre el servidor web y las aplicaciones FastCGI. Gestiona las solicitudes entrantes, y se las pasa a la aplicación FastCGI correspondiente y devuelve las respuestas al servidor web.

```
sudo apt-get install nginx fcgiwrap
```

Una vez instalado el paquete vamos a copiar  el archivo /usr/share/doc/fcgiwrap/examples/nginx.conf   en el directorio de configuración de nginx, o sea vamos a crear un archivo de configuración para la puerta de enlace CGI.

<pre><code><strong>sudo cp /usr/share/doc/fcgiwrap/examples/nginx.conf /etc/nginx/fcgiwrap.conf
</strong></code></pre>

## El directorio cgi-bin

Vamos a crear un directorio para almacenar nuestros archivos CGI.

```
mkdir /var/www/punky.com/cgi-bin -p
```

<figure><img src="../.gitbook/assets/image (307).png" alt=""><figcaption><p>Directorio /var/www/punky.com/cgi-bin/ para almacenar nuestros archivos cgi</p></figcaption></figure>

### Configurando nginx

Editamos el archivo de configuración /etc/nginx/sites-available/punky.conf para el sitio web predeterminado.

```
sudo nano /etc/nginx/sites-available/punky.conf
```

Y vamos a insertamos la siguiente línea al final del archivo:

```
include /etc/nginx/fcgiwrap.conf;
```

<figure><img src="../.gitbook/assets/image (308).png" alt=""><figcaption><p>Archivo de configuración /etc/nginx/sites-available/punky.conf</p></figcaption></figure>

### Adaptando el archivo /etc/nginx/fcgiwrap.conf

Si vamos a almacenar nuestros archivos cgi dentro de un directorio en la carpeta de nuestro sitio, tenemos que modificar el fcgiwrap.conf para que autorice la ejecución de esos archivos. Para realizar esos cambios lo que tenemos que hacer es escribir nuestra "location" para que todo apunte al directorio `cgi-bin`:

<figure><img src="../.gitbook/assets/image (309).png" alt=""><figcaption><p>Archivo /etc/nginx/fcgiwrap.conf </p></figcaption></figure>

### Creando un CGI de prueba

Ahora necesitamos crear un contenido de prueba. Para ello vamos a escribir un pequeño script en C que mostrará una página web estática. Escribimos:

```
nano /var/www/punky.com/cgi-bin/test.c
```

Y escribimos nuestro pequeño script en C:

```
#include <stdio.h>
int main()
{
  printf("Content-type: text/html\n\n");
  printf("<html>\n");
  printf("<body>\n");
  printf("<h1>Hello there!</h1>\n");
  printf("</body>\n");
  printf("</html>\n");
  return 0;
}
```

Ahora solo nos queda compilar el código para generar un archivo ejecutable:

```
gcc test.c -o test.cgi
```

Le damos permisos de ejecución al archivo creado:

```
sudo chown www-data:www-data /var/www/punky.com/cgi-bin/test.cgi
sudo chmod 755 /usr/lib/cgi-bin/test.cgi
```

### Visualizando el cgi

Ahora nos vamos al navegador en el equipo cliente y escribimos la dirección IP del servidor web y le agregamos `/cgi-bin/test.cgi`. Esto es:

```
http://192.168.6.100/cgi-bin/test.cgi
```

Y se nos mostrará el contenido escrito en C.

<figure><img src="../.gitbook/assets/image (310).png" alt=""><figcaption><p>Nuestro CGI escrito en C</p></figcaption></figure>

## Creando otro CGI de prueba

Esta vez vamos a crear un CGI de prueba pero con PHP. Vamos a editar un archivo llamado hello.cgi.

```
nano /var/www/punky.com/cgi-bin/hello.cgi
```

Y escribimos algo sencillo como lo siguiente:

```
#!/usr/bin/php
<?php
echo "Content-type: text/html\r\n\r\n";
echo "<html><head><title>CGI de prueba</title></head><body>";
echo "<h1>Hola CGI</h1>";
echo "</body></html>";
?>
```



<figure><img src="../.gitbook/assets/image (312).png" alt=""><figcaption><p>CGI de prueba /var/www/punky.com/cgi-bin/hello.cgi</p></figcaption></figure>

Le damos permisos de ejecución al archivo creado en php:

```
sudo chown www-data:www-data /var/www/punky.com/cgi-bin/hello.cgi
sudo chmod 755 /usr/lib/cgi-bin/hello.cgi
```

### Visualizando el CGI en PHP

Ahora nos vamos al navegador en el equipo cliente y escribimos la dirección IP del servidor web y le agregamos `/cgi-bin/hello.cgi`. Esto es:

```
http://192.168.6.100/cgi-bin/hello.cgi
```

Y se nos mostrará el contenido escrito en PHP.

<figure><img src="../.gitbook/assets/image (313).png" alt=""><figcaption><p>Nuestro CGI escrito en PHP</p></figcaption></figure>

## Links

* Para instalar MySQL tienes este tutorial: [https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-22-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-22-04)&#x20;
* [https://techexpert.tips/es/nginx-es/nginx-shell-script-cgi/](https://techexpert.tips/es/nginx-es/nginx-shell-script-cgi/)
