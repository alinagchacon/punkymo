---
description: Pi-hole
---

# Instalando Pi-hole

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



### Links

* [https://docs.pi-hole.net/main/basic-install/](https://docs.pi-hole.net/main/basic-install/)
