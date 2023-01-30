---
description: de  docker-compose
---

# Ejemplo

Utilicemos a modo de ejemplo la creación de varios contenedores relacionados entre sí, para levantar un sitio web en PHP, que se conecta a una DB en MySQL, de hecho, el LoginRegister hecho en 1º año.&#x20;

La actividad de LoginRegister tiene una estructura de archivos como se muestra en la imagen siguiente. A tener en cuenta que los directorios <mark style="color:blue;">`log`</mark> y <mark style="color:blue;">`mysql`</mark> no son necesarios.

<figure><img src="../../../../.gitbook/assets/image (13).png" alt=""><figcaption><p>Estructura de archivos .php de la aplicación web</p></figcaption></figure>

Para ello necesitamos los servicios activos de:

* **Apache** o **Nginx** para el servidor web
* **MySql** para la base de datos
* **PHP** para el lenguaje de programación
* **Phpmyadmin**, si quiero conectarme a un gestor de DB.

Veamos cómo levantar un <mark style="color:blue;">`docker-compose`</mark> creando un archivo de configuración <mark style="color:blue;">`docker-compose.yml`</mark>, dado que el <mark style="color:blue;">`docker-compose`</mark> busca, por defecto,  las instrucciones en el archivo <mark style="color:blue;">`docker-compose.yml`</mark>.

#### **Estructura del archivo**

Sin embargo, hay que tener en cuenta la estructura y las variables que podemos utilizar para definir los servicios, puertos, etc.:

* **version 3.8**: Es muy importante indicar la versión de las instrucciones que vamos a utilizar. Docker evoluciona, pero siempre hay compatibilidad con las versiones anteriores.
* **app**: Indica el nombre del servicio, que podría ser cualquiera. El nombre indica el tipo de servicio que estamos construyendo. Por ejemplo: MySQL, NGINX, etc.
* **build**:  Indica dónde está el Dockerfile a utilizar para crear el contenedor. Si escribimos <mark style="color:blue;">`build .`</mark> se considera que el Dockerfile está en el directorio actual.
* **ports**: Aquí escribimos los puertos locales que utilizaremos. &#x20;
* **volumes**: Aquí hacemos un mapeo entre el directorio del host y el directorio del contenedor. De este modo, cualquier cambio en el directorio local en el host, se hará de inmediato en el contenedor.

A continuación, hay un ejemplo del <mark style="color:blue;">`docker-compose.yml`</mark> que he utilizado:

<figure><img src="../../../../.gitbook/assets/image (20).png" alt=""><figcaption><p>Archivo docker-compose.yml</p></figcaption></figure>

<mark style="color:red;">to be continued ...</mark>
