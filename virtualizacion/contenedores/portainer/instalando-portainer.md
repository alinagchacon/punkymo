---
description: Instalando Portainer
---

# Instalando Portainer

### Instalar  Portainer&#x20;

Portainer consta de dos elementos: `Portainer Server` y `Portainer Agent`. Estos elementos se ejecutan como contenedores Docker.&#x20;

Vamos con la instalación del contenedor de Portainer Server. Para ello toma nota del sitio siguiente: [https://docs.portainer.io/start/install-ce/server/docker/linux](https://docs.portainer.io/start/install-ce/server/docker/linux).&#x20;

Entre los requisitos que necesitamos tener en consideración están:

* La última versión de Docker instalada y funcionando.&#x20;
* Acceso de super usuario (sudo) en la VM que alojará la instancia de Portainer Server&#x20;

De forma predeterminada, Portainer Server accede a la interfaz de usuario a través del puerto `9443` y expondrá un servidor de túnel TCP a través del puerto `8000`. Esto último es opcional.

Lo primero que tenemos que hacer es crear el volumen que Portainer Server usará para almacenar su base de datos.

<pre class="language-bash"><code class="lang-bash"><strong>docker volume create portainer_data
</strong></code></pre>

Si quieres verificar el volume recién creado puedes escribir:

<pre><code><strong>docker volume inspect portainer_data
</strong></code></pre>

Y verás algo como lo siguiente:

<figure><img src="../../../.gitbook/assets/image (43).png" alt=""><figcaption><p>Inspeccionando un volumen de docker</p></figcaption></figure>

Ahora, descargamos e instalamos el contenedor de Portainer Server:

```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
--restart=always -v \
/var/run/docker.sock:/var/run/docker.sock -v\
portainer_data:/data \
portainer/portainer-ce:latest
```

Realmente, es toda una línea:&#x20;

```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

Al ejecutar esta línea de comando nos dice que no puede encontrar la imagen de Portainer en el sistema, con lo cual la descarga la última imagen existente de portainer/portainer -ce.

<figure><img src="../../../.gitbook/assets/image (211).png" alt=""><figcaption><p>Instalando Portainer en Docker</p></figcaption></figure>

De forma predeterminada, Portainer genera y utiliza un certificado SSL auto-firmado para asegurar el puerto 9443. Alternativamente, puede proporcionar su propio certificado SSL durante la instalación o mediante la interfaz de usuario de Portainer, una vez que se completa la instalación.

El servidor de Portainer ha sido instalado. Puedes comprobar si el contenedor del servidor de Portainer se ha iniciado ejecutando docker ps:

```
docker ps
```

<figure><img src="../../../.gitbook/assets/image (218).png" alt=""><figcaption><p>Comprobando el servidor de Portainer</p></figcaption></figure>

Si necesitas reiniciar el servicio de Portainer haríamos:

```
docker restart portainer
```

### Logging In

Ya se completó la instalación, con lo cual podemos iniciar sesión en la instancia de Portainer Server abriendo un navegador web y yendo a:

```
https://IP_de_VM:9443
```

<figure><img src="../../../.gitbook/assets/image (200).png" alt=""><figcaption><p>Accediendo al servicio de Portainer en la VM de Alpine </p></figcaption></figure>

Una vez dentro, ya podemos comenzar a utilizar Portainer.



<figure><img src="../../../.gitbook/assets/image (1) (4).png" alt=""><figcaption><p>Accediendo a Portainer, recién instalado</p></figcaption></figure>

### Instalando Pi-hole

Busca la imagen de Pi-Hole en local y al no encontrarla la descarga del sitio oficial de Pi-Hole de Docker.&#x20;

```
docker search pihole/pihole:latest
docker pull pihole/pihole:latest 
```

Para ver las **imágenes descargadas** ejecuta:

```
docker images
```

Para e**jecutar** el contenedor de pihole:

```
docker run -it pihole/pihole:latest
```

Realmente ejecuta el comando en un nuevo contenedor. Este comando **run** primero crea una capa de contenedor en la que se puede escribir sobre la imagen especificada y luego la inicia con el comando especificado.

Este ejemplo, ejecuta un contenedor usando la imagen más reciente de pihole. La combinación de las opciones **-i** y **-t** brindan acceso interactivo del Shell al contenedor.

Nota: Pudiera ser que no funcionara a la primera. Entonces quizá sea mejor utilizar un docker-compose.yml con el siguiente contenido:

```
version: "3"

# More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
      - "80:80/tcp"
    environment:
      TZ: 'Europe/Berlin'
      WEBPASSWORD: elquesea
    # Volumes store your data between container upgrades
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    #   https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
    cap_add:
      - NET_ADMIN # Required if you are using Pi-hole as your DHCP server, else not needed
    restart: unless-stopped
```

### Accediendo a Pi-Hole

Para acceder a nuestro contenedor de pihole basta usar el navegador, escribiendo la IP de nuestra MV. Esto es: <mark style="color:blue;">`http://192.168.1.79`</mark>

o h[ttp://192.168.1.79/admin/index.php](http://192.168.1.34/admin/index.php)

### Configurando Portainer

_**Portainer**_  es un conjunto de herramientas de administración de código abierto que nos permite construir, administrar y mantener entornos _Docker de un modo más cómodo._

Nos vamos a <mark style="color:blue;">`https://192.168.1.79:9443`</mark> y seleccionamos el modo de environment:

<figure><img src="../../../.gitbook/assets/image (203).png" alt=""><figcaption></figcaption></figure>
