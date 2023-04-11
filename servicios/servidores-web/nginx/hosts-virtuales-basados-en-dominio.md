---
description: SSL
---

# Hosts virtuales basados en dominio

El contexto del servidor permite que varios dominios o sitios web se puedan almacenar y sirvan desde la misma máquina física o servidor privado virtual (VPS). En Nginx al igual que en Apache, se pueden declarar varios bloques de servidor (que representan hosts virtuales) dentro del contexto http para cada sitio/dominio. Nginx decide qué servidor procesa una solicitud en función del encabezado de solicitud que recibe.

Vamos a probar esta idea para lo cual utilizaremos dominios ficticios.

### Dominios – directorios de los sitios web

Creamos los dominios/directorios que contendrán los sitios web, cada uno ubicado en su directorio específico:

* kirby.com - /var/www/kirby.com/
* punky.com - /var/www /punky.com/

A continuación, le asignamos los permisos correspondientes en el directorio de cada sitio.

```
$ sudo chmod -R 755 /var/www/ kirby.com
$ sudo chown -R www-data:www-data /var/www/ kirby.com
$ sudo chmod -R 755 /var/www/ punky.com
$ sudo chown -R www-data:www-data /var/www/ punky.com
```

### Creamos una página de prueba en cada sitio

Creamos el archivo index.html en cada sitio web, con fines de prueba. Este archivo ofrecerá contenido que se mostrará en el navegador web cuando se llame al dominio en el navegador. Por ejemplo:

```
$ sudo nano /var/www/kirby.com/index.html
```

Puedes pegar el siguiente contenido HTML.

```
<html>
    <head>
        <title>Welcome to Kirby!</title>
    </head>
    <body>
  <h1>¡Welcome to Kirby en Kirby - Ubuntu Desktop!</h1>
    </body>
</html>
```

Copia el archivo <mark style="color:blue;">`index.html`</mark> y pega en el otro directorio, en mi caso, en <mark style="color:blue;">`/var/www/punky.com`</mark>. Haz alguna modificación en el documento HTML para probar uno y otro sitio web.

### Crear bloques en el servidor Nginx

Tenemos que crear los bloques del servidor Nginx en el directorio <mark style="color:blue;">`/etc/nginx/sites-available`</mark>. Recuerda que el bloque es <mark style="color:blue;">`/etc/nginx/sites-available/default`</mark> que sirve el archivo HTML predeterminado en <mark style="color:blue;">`/var/www/html/index.nginx-debian.html`</mark>. Justamente la web que nos muestra cuando instalamos el Nginx y escribimos localhost en el navegador.

En nuestro caso, vamos a tener que crear un bloque que sirva el contenido de los sitios previamente creados. Para ello, podemos copiar y pegar el archivo por defecto <mark style="color:blue;">`/etc/nginx/sites-available/default`</mark> como:

```
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/kirby.com.conf
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/punky.com.conf
```

El contenido de ambos ficheros sería:

```
server {
        listen 80;
        listen [::]:80;
        root /var/www/kirby.com;
        index index.html;
        server_name kirby.com www.kirby.com;

        location / {
                try_files $uri $uri/ =404;
        }

        location /images/{
                root /img;
        }
        
        access_log /var/log/nginx/kirby.com.access.log;
        error_log /var/log/nginx/kirby.com.error.log;
}
```

#### Habilitando los bloques en ../sites-enable/

Para habilitar el bloque del servidor Nginx, éste debe ser vinculado simbólicamente al directorio <mark style="color:blue;">`/etc/nginx/sites-enabled/`</mark>. Para ello, creamos un enlace simbólico dentro de la carpeta sites-enable:

```
$ sudo ln -s /etc/nginx/sites-available/kirby.com /etc/nginx/sites-enabled/
$ sudo ln -s /etc/nginx/sites-available/punky.com /etc/nginx/sites-enabled/
```

Verificamos que la configuración de nginx está correcta haciendo:

```
$ sudo nginx –t                  
```

Para reiniciar el nginx podemos hacer:

```
$ sudo nginx –s reload
$ sudo systemctl restart nginx
```

#### Testeando

Para confirmar si el bloque del servidor está funcionando como se esperaba y está sirviendo contenido en el directorio /var/www/kirby.com y en <mark style="color:blue;">`/var/www/punky.com`</mark>, abre el navegador y busca el nombre de dominio: [<mark style="color:blue;">`http://www.kirby.com`</mark>](http://www.kirby.com)

Debe obtener el contenido incluido en el archivo HTML en su bloque de servidor como se muestra.

<figure><img src="../../../.gitbook/assets/image (23) (2).png" alt=""><figcaption><p>www.kirby.com </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (22) (1).png" alt=""><figcaption><p>www.punky.com </p></figcaption></figure>

## SSL en Nginx - OPENSSL

Los certificados SSL ayudan a habilitar http seguro (HTTPS) en su sitio, que es esencial para establecer una conexión confiable/segura entre los usuarios finales y su servidor al cifrar la información que se transmite hacia o desde el sitio web.

Vamos a ver cómo crear e instalar un certificado <mark style="color:blue;">`autofirmado`</mark> y generar una solicitud de firma de certificado (CSR) para adquirir un certificado SSL de una autoridad de certificación (CA), y poder utilizarlo con Nginx.

Los certificados auto firmados se pueden crear de forma gratuita y prácticamente están listos para realizar pruebas y para servicios internos de redes LAN. Cuando se trata de servidores públicos, se recomienda utilizar un certificado emitido por una CA (Let's Encrypt) para mantener su autenticidad.

Para crear un certificado auto firmado, lo primero que debemos hacer es crear un directorio que nos permita almacenarlos.

```
$ sudo mkdir /etc/nginx/ssl-certs/
```

Posteriormente, generamos el certificado autofirmado y la clave usando la herramienta de línea de comando openssl.

```
$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl-certs/kirby.com.key -out /etc/nginx/ssl-certs/kirby.com.crt
```

&#x20;Donde:

* **req -X509**: muestra que estamos creando un certificado x509.
* **nodes** (NO DES): significa "no cifrar la clave".
* **days 365**: especifica la cantidad de días durante los que será válido el certificado.
* **newkey rsa**: 2048: especifica que la clave generada mediante el algoritmo RSA debe ser de 2048 bits.
* **keyout** /etc/nginx/ssl-certs/kirby.com.key: especifica la ruta completa de la clave RSA.
* **out** /etc/nginx/ssl-certs/kirby.com.crt: especifica la ruta completa del certificado.

### Directivas en el archivo de configuración de kirby.com

Dentro del archivo de configuración:  <mark style="color:blue;">`/etc/nginx/sites-available/kirby.com.conf`</mark> agregaremos las siguientes directivas:

```
listen 443 ssl;
ssl_certificate /etc/nginx/ssl-certs/kirby.com.crt;
ssl_trusted_certificate /etc/nginx/ssl-certs/kirby.com.crt;
ssl_certificate_key /etc/nginx/ssl-certs/kirby.com.key;
```

Las directivas <mark style="color:blue;">`ssl_protocols`</mark> y <mark style="color:blue;">`ssl_ciphers`</mark> se pueden usar para limitar las conexiones para incluir solo las versiones seguras y cifrados de SSL/TLS.

De forma predeterminada, Nginx usa <mark style="color:blue;">`ssl_protocols`</mark> TLSv1 TLSv1.1 TLSv1.2 y <mark style="color:blue;">`ssl_ciphers`</mark> HIGH:!aNULL:!MD5, por lo que generalmente no es necesario configurarlos explícitamente.

&#x20;

<figure><img src="../../../.gitbook/assets/image (14) (1) (1).png" alt=""><figcaption><p>COnfiguración del archivo de configuración de kirby.com </p></figcaption></figure>

Para probar que funciona correctamente el certificado creado con openssl, basta ir al navegador y llamar a nuestros sitios web: <mark style="color:blue;">`https://www.kirby.com`</mark> y <mark style="color:blue;">`https://www.punky.com.`</mark>  Como se muestra en las imágenes a continuación.

<figure><img src="../../../.gitbook/assets/image (3) (3).png" alt=""><figcaption><p>https://www.kirby.com</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (6) (4).png" alt=""><figcaption><p>https://www.punky.com</p></figcaption></figure>

<mark style="color:red;">To be continued ...</mark>

## LINKS

* [https://techexpert.tips/es/nginx-es/nginx-virtualhost-varios-sitios-web-en-el-mismo-servidor/](https://techexpert.tips/es/nginx-es/nginx-virtualhost-varios-sitios-web-en-el-mismo-servidor/)
* [Beginner’s Guide (nginx.org)](https://nginx.org/en/docs/beginners\_guide.html)
* [https://nginx.org/en/docs/http/configuring\_https\_servers.html](https://nginx.org/en/docs/http/configuring\_https\_servers.html)
* [https://www.digicert.com/kb/ssl-support/openssl-quick-reference-guide.htm](https://www.digicert.com/kb/ssl-support/openssl-quick-reference-guide.htm)&#x20;
