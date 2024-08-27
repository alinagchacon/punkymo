---
description: Configuración de un proxy en Nginx
---

# Proxy reverse Nginx

En este tutorial vamos a configurar un servidor Ubuntu con Nginx como un proxy para un servidor que también tiene instalado un servidor web con Nginx en Ubuntu Linux. Dicho en otras palabras, cuando solicitemos ver la página www.kirby.com de un servidor veremos la página que existe en otro servidor.

Las tres VM que voy a utilizar para testear son:

| Equipo              | IP                   |
| ------------------- | -------------------- |
| Ubuntu Server 23.10 | 192.168.2.4          |
| Debian 11           | 192.168.2.110 (DHCP) |
| Debian 11           | 192.168.2.105 (DHCP) |

El siguiente diagrama de red muestra lo que estoy diciendo:

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Diagrama de la red</p></figcaption></figure>

Las tres VM  están en adaptador Red NAT. El Ubuntu Server (verde) hace de servidor de DNS y DHCP así como dos sitios web: www.kirby.com y www.punky.com.  Para el sitio web kirby.com he habilitado el módulo de PHP. El sitio web punky.com es solo una página web estática.&#x20;

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>www.kirby.com y www.punky.com del servidor Ubuntu</p></figcaption></figure>

En la VM con Debian (azul) que tiene IP 192.168.2.110 tenemos un sitio web también configurado con Nginx. De hecho, es una VM utilizada como DMZ mientras hacía pruebas con el firewall pfSense.

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1).png" alt="" width="359"><figcaption><p>La web de prueba en el servidor Debian</p></figcaption></figure>

La tercera VM, el servidor de Debian (lila) la utilizaré solo para acceder a los sitios web de los otros dos servidores.

### Configurando el server Ubuntu como proxy

Dado que ya tengo configurado el servicio web con Nginx en el equipo de Ubuntu Server, solo me queda agregar un par de parámetros en la configuración, así que vamos allá. Como se puede ver en la imagen a continuación, el único parámetro que he agregado a la configuración del sitio web: www.kirby.com en Nginx es:

```
proxy_pass http://192.168.2.110:80
include proxy_params;
```

Este parámetro: `proxy_pass` se usa en Nginx para configurar un servidor proxy inverso. Dicho  parámetro indica la dirección del servidor backend al que Nginx debe pasar las solicitudes entrantes, o sea, el parámetro `proxy_pass` especifica la URL a la que se deben enviar las solicitudes HTTP o TCP recibidas por el servidor Nginx para que el `proxy inverso` las reenvíe al servidor `backend` correspondiente.&#x20;

En nuestro caso, significa que todas las solicitudes recibidas por Nginx bajo la ubicación `/` serán enviadas al servidor backend especificado por `http://192.168.2.110:80`.

Por lo tanto, el `proxy_pass` es una herramienta  importante a la hora de configurar Nginx cuando se quiere implementar un `servidor de proxy inverso`, que  es utilizada para equilibrar la carga de tráfico web, enmascarar la dirección IP del servidor backend, etc.

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Configuración de Nginx para el sitio www.kirby.com</p></figcaption></figure>

Una vez se haya modificado nuestro archivo de configuración correspondiente a www.kirby.com reiniciamos el servicio de Nginx:

```
sudo systemctl restart nginx
```

En nuestro ejemplo, el servidor Nginx en Ubuntu funcionará como proxy y enviará todas las solicitudes al servidor remoto que, en mi caso, es el Debian con IP 192.168.2.110. Vamos a comprobarlo desde a VM Debian con IP 192.168.2.105. Nos vamos al navegador y llamamos a www.kirby.com y en lugar de visualizar el contenido de dicho sitio veremos el contenido de la página web del otro servidor de Debian con IP 192.168.2.110.&#x20;

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>La página web del server 192.168.2.110 cuando se solicita la página de www.kirby.com del server 192.168.2.4</p></figcaption></figure>

Si eres uno de mis alumnos te aseguro que tienes todos los elementos para probar este ejemplo y que no te lleve más de 30 minutos como mucho **:-)**

