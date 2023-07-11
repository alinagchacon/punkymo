---
description: N-Tech-Admin Group
---

# Alpine Linux

En este apartado vamos a utilizar una VM con Alpine Linux por su facilidad de uso y por ser una distribución ultra ligera para instalar Docker, Portainer para facilitar el trabajo con Docker y Pi-Hole.

### Por qué Alpine

Una versión no GNU que puede llegar a ocupar tan solo 8MB... :smile:

Se trata de:&#x20;

* una distribución Linux ultraligera
* orientada a la seguridad y cuyo software se compila usando Musl en lugar de la biblioteca C de GNU (Libc)
* sustituye las herramientas GNU por Busybox, un único ejecutable capaz de emular el funcionamiento de todas ellas.
* Ha sido diseñada principalmente para Routers x86, Firewalls, VPN, VoIP y servidores.&#x20;
* Se utiliza en VM o  Docker, para ejecutar **entornos de desarrollo y ejecución de aplicaciones, portables y aislados del sistema principal**
* También **es posible instalar las herramientas GNU**, así como softwares como **KDE, GNOME**&#x20;
* Para administrar el software Alpine recurre a su propio gestor de paquetes: **APK**.

### Algunos comandos básicos

Para actualizar las listas de paquetes:

```
apk update
```

```
apk upgrade
```

Para desinstalar un paquete:

```
apk del name_package
```

Para pedir ayuda:

<pre><code><strong>apk --help
</strong></code></pre>

Para ver todos los paquetes:

```
apk search -v
```

Para buscar un paquete en específico:

```
apk search -v nodejs
```

```
apk search -v | grep -i nodejs
```

Para instalar paquetes:

```
apk add package
```

Para enumerar los paquetes instalados:

```
apk info
```

Para verificar si un paquete específico está instalado:

```
apk info package-name
```

Reiniciar el sistema, esto es, el equivalente a <mark style="color:blue;">`shutdown now -r`</mark> sería:

```
reboot
```

Apagar el sistema, el equivalente a: <mark style="color:blue;">`shutdown now -P`</mark>:

```
poweroff
```

Renovar los parámetros de red:

```
service networking restart
```

### Instalando Alpine Linux

A la hora de instalar la MV tenemos que tener en cuenta las necesidades específicas. Por ello, siempre es conveniente que detallemos los requisitos de la misma.

#### Requisitos técnicos

**RAM**: 4GB

**Tamaño disco**: 20GB

**Sistema Operativo**: Linux / Linux 2.6 / 3.x / 4.x (64-bit)

**Alpine System**: alpine-virt-3.16.2-x86\_64.iso



Iniciamos la MV y lo primero que nos encontramos es que nos pide el usuario del sistema, sin haber hecho la instalación. Para acceder, basta con escribir <mark style="color:blue;">`root`</mark>.

Leyendo la información que nos brinda en pantalla vemos que nos dice que para instalar el sistema escribamos:

```
setup-alpine
```

Dado que el sistema está en inglés, tengamos en cuenta la posición de los caracteres en el teclado. No obstante, lo primero que nos solicita es nuestro idioma y distribución. Escribimos <mark style="color:blue;">`es`</mark> las dos veces que nos lo pide.&#x20;

<figure><img src="../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

Podemos dejar los valores por defecto:

* localhost
* eth0
* dhcp
* configuración manual de la red: N
* el password que deseemos
* podemos ignorar la zona horaria
* proxy no tenemos
* NTP client: chrony (por defecto)
* mirror server: seleccionemos la opción por defecto: 1
* user: lo dejamos por defecto
* ssh: openssh para que nos lo instale&#x20;
* permitir que el root se conecte por ssh (no me gusta por seguridad pero ...)
* disco a utilizar: sda, tenemos un solo disco. En caso de duda podemos teclear ? y nos dice las opciones.
* &#x20;configuración base del sistema para el arranque en uno de los tres modos de disco diferentes de Alpine Linux: 'sin disco', 'datos' o 'sys'. Seleccionamos sys.

Una vez hecho esto, comienza la instalación del sistema que dura segundos y nos pedirá reiniciar el sistema. Mejor lo apagamos para quitar la ISO y no vuelva al punto de partida de la instalación.

Para poder utilizar todos los paquetes disponibles para Alpine debemos hacer una modificación en el archivo de los repositorios, el equivalente al sources.list de Ubuntu que contiene los repositorios del tipo Main.

Vamos al fichero: <mark style="color:blue;">`/etc/apk/repositories`</mark> y lo editamos pero mejor que instalemos previamente el nano, así que lo instalamos:

```
apk add nano
```

Abrimos el archivo de los repositorios y eliminamos el <mark style="color:blue;">`#`</mark> de las líneas que vienen comentadas. Éstas son: main, community y testing.

```
nano /etc/apk/repositories
```

Actualizamos los paquetes:&#x20;

```
apk update
apk upgrade
```

### Instalando Docker y Portainer&#x20;

#### Docker

Comenzamos por instalar Docker y para ello basta con:

```
apk add docker
apk add docker-compose
```

Para iniciar el servicio de Docker daemon al inicio:

```
rc-update add docker boot 
service docker start
```

Para comprobar el estado del servicio:

```
service docker status
```

En el caso de que el servicio estuviera apagado, lo iniciamos y comprobamos nuevamente:

```
service docker start
service docker status
```

#### Portainer

Ahora vamos con la instalación de Portainer. Para ello toma nota del sitio siguiente:

{% embed url="https://docs.portainer.io/v/ce-2.9/start/install/server/docker/linux" %}

Creamos el volumen que Portainer Server usará para almacenar su base de datos:

<pre class="language-bash"><code class="lang-bash"><strong>docker volume create portainer_data
</strong></code></pre>

Si quieres verificar el volume recién creado puedes escribir:

<pre><code><strong>docker volume inspect portainer_data
</strong></code></pre>

Y verás algo como lo siguiente:

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption><p>Inspeccionando un volumen de docker</p></figcaption></figure>

Ahora, descargamos e instalamos el contenedor de Portainer Server:

```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
--restart=always -v \
/var/run/docker.sock:/var/run/docker.sock -v\
portainer_data:/data \
portainer/portainer-ce:2.9.3
```

Realmente, es toda una línea:&#x20;

```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

Al ejecutar esta línea de comando nos dice que no puede encontrar la imagen de Portainer en el sistema, con lo cual la descarga la última imagen existente de portainer/portainer -ce.

<figure><img src="../../.gitbook/assets/image (211).png" alt=""><figcaption><p>Instalando Portainer en Docker</p></figcaption></figure>

De forma predeterminada, Portainer genera y utiliza un certificado SSL auto-firmado para asegurar el puerto 9443. Alternativamente, puede proporcionar su propio certificado SSL durante la instalación o mediante la interfaz de usuario de Portainer, una vez que se completa la instalación.

El servidor de Portainer ha sido instalado. Puedes comprobar si el contenedor del servidor de Portainer se ha iniciado ejecutando docker ps:

```
docker ps
```

<figure><img src="../../.gitbook/assets/image (218).png" alt=""><figcaption><p>Comprobando el servidor de Portainer</p></figcaption></figure>

Si necesitas reiniciar el servicio de Portainer haríamos:

```
docker restart portainer
```

Nos vamos al navegador y escribimos la IP de la VM y el puerto de acceso:

<mark style="color:blue;">`https://192.168.1.79:9443`</mark>\


<figure><img src="../../.gitbook/assets/image (200).png" alt=""><figcaption><p>Accediendo al servicio de Portainer en la VM de Alpine </p></figcaption></figure>

Una vez dentro, ya podemos utilizar el Portainer.



<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Accediendo a Portainer, recién instalado</p></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (203).png" alt=""><figcaption></figcaption></figure>

E instalamos Docker Standalone

<figure><img src="../../.gitbook/assets/image (214).png" alt=""><figcaption></figcaption></figure>

Podemos visualizar nuestra  lista de contenedores creados. En este caso, pihole y portainer:

<figure><img src="../../.gitbook/assets/image (220).png" alt=""><figcaption><p>Container</p></figcaption></figure>

Si clicas en uno de los contenedores se desplegarán mas opciones específicas:

<figure><img src="../../.gitbook/assets/image (205).png" alt=""><figcaption></figcaption></figure>

### Algunos aspectos a considerar&#x20;

Network Time Protocol (NTP) es un protocolo cliente-servidor. Un dispositivo cliente puede solicitar periódicamente información de tiempo de un servidor NTP y éste responde a las solicitudes con información de tiempo que el cliente puede usar para fines de sincronización.

La mayoría de los SO vienen con herramientas integradas para la sincronización automática, sin embargo, existen herramientas de terceros que brindan funciones mejoradas y mejor sincronización.



### Links

#### Alpine Linux&#x20;

* [https://alpinelinux.org/downloads/](https://alpinelinux.org/downloads/)
* [https://wiki.alpinelinux.org/wiki/Installation](https://wiki.alpinelinux.org/wiki/Installation)

#### Docker

* [https://wiki.alpinelinux.org/wiki/Docker](https://wiki.alpinelinux.org/wiki/Docker)
* [https://docs.linuxserver.io/images/docker-swag#site-config-and-reverse-proxy](https://docs.linuxserver.io/images/docker-swag#site-config-and-reverse-proxy)

#### Portainer

* [https://docs.portainer.io](https://docs.portainer.io)

#### Pi-hole

* [https://github.com/pi-hole/docker-pi-hole](https://github.com/pi-hole/docker-pi-hole)

#### Misceláneas

* [https://www.linuxserver.io](https://www.linuxserver.io)
* [https://timetoolsltd.com/ntp/ntp-client/](https://timetoolsltd.com/ntp/ntp-client/)
* [https://www.netmentor.es/entrada/introduccion-portainer-contenedores](https://www.netmentor.es/entrada/introduccion-portainer-contenedores)&#x20;



