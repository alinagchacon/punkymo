# En Alpine linux

Algunos apuntes a la hora de configurar Nginx en Alpine linux.&#x20;

Para instalar nginx bastaría:

```
apk add nginx
```

Se instala igualmente en: **/etc/nginx/** y los archivos de configuración a tener en cuenta sería: **nginx.conf** y **http.d/default.conf**:

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1).png" alt="" width="563"><figcaption><p>Archivos de configuración de /etc/nginx</p></figcaption></figure>

Muchos sitios hacen referencia a **/usr/share/nginx/** para alojar el sitio web pero podemos hacer uso del directorio habitual: **/var/www/**. &#x20;

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>La web estática</p></figcaption></figure>

Más que configurar nginx para brindar páginas estáticas, lo suyo es brindar páginas dinámicas con PHP. Para ello necesitamos instalar tanto **php** como **php-fpm** que, usado conjuntamente con un servidor web como Apache o Nginx, se encarga de servir el contenido dinámico, mientras el servidor web (Apache o Nginx) se encarga de servir el contenido estático.

Algunos detalles a considerar:

* Nos aseguramos de tener activo el repositorio **community**. Podemos hacer también un **update** de los paquetes.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>/etc/apk/repositories</p></figcaption></figure>

* Instalamos PHP y PHP-FPM:

```
sudo apk add php8.3 php8.3-fpm
```

* Debemos tener en cuenta las directivas de configuración de php:

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt="" width="335"><figcaption><p>Directivas de configuración de php</p></figcaption></figure>

Podemos ver conocer el valor de la directiva **listen** ejecutando el comando:

```plaintext
cat /etc/php83/php-fpm.d/www.conf | grep 'listen = '
listen = 127.0.0.1:9000
```

Una vez tenemos instalado php-fpm comprobamos que esté activo:

```
rc-service php-fpm83 status

en caso de estar detenido, lo activamos:

rc-service php-fpm8 start
```

Podemos verificar que los cambios en php-fpm se realizan de manera correcta haciendo:

```
sudo php-fpm83 -t
```

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Salida del comando php-fpm83 -t </p></figcaption></figure>

Y refrescamos la configuración con:

```
sudo rc-service php-fpm83 reload
```

### Configurando una página web con php

Para configurar una página web de prueba podemos hacer lo siguiente:

```
echo "<?php phpinfo(); ?>" > /var/www/lore/index.php
```

Veamos ahora la configuración de **/etc/nginx/http.d/default**. Como tenía en uso el puerto 80 habilité el 84 para Nginx. Siempre me gusta habilitar los logs, así que eso es lo que hice. Por otra parte,  PHP-FPM esperará las conexiones en el puerto **9000** de localhost. **Nginx** envía las solicitudes PHP a PHP-FPM a través del protocolo **FastCGI**.

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption><p>Archivo de configuración /etc/nginx/http.d/default.conf</p></figcaption></figure>

Desde el navegador podemos ver la página web, escribiendo: [http://192.168.1.80:84](http://192.168.1.80:84)&#x20;

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>
