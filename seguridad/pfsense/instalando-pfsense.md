---
description: Instalación básica
---

# Instalando Pfsense

### ¿Dónde lo descargo?



### Instalación&#x20;

Vamos hacer la instalación en una VM en VirtualBox. Utilizaremos dos adaptadores de red:

1. Adaptador de red: NAT
2. Adaptador de red: solo anfitrión

Para instalar Pfsense en la MV voy a utilizar la siguiente configuración:

* RAM:  2048
* HDD:  16 GB
* S.O.:  BSD

Instalamos todo por defecto. Apagamos. Quitamos la ISO y reiniciamos. Una vez que nos sale la pantalla inicial, comenzamos a configurar.

<figure><img src="../../.gitbook/assets/image (258).png" alt=""><figcaption><p>PFsense instalado en la VM</p></figcaption></figure>

En el menú de pfsense podemos ver cada una de las opciones que nos brinda y que podemos configurar, aunque lo adecuado es acceder  a la web para configurar los servicios.

* WAN -> em0 -> 10.0.2.15/24
* LAN -> em1 -> 192.168.56.2/24
* DHCP -> from 192.168.56.100 TO 192.168.56.105

### &#x20;Diagrama de red

Vamos a configurar un escenario muy básico.&#x20;

<figure><img src="../../.gitbook/assets/image (256).png" alt=""><figcaption><p>Firewall con dos adaptadores de red</p></figcaption></figure>

### Configuración básica

Como podemos observar en el diagrama de red y en las opciones del menú una vez instalado el pfsense la IP que nos ha asignado para el adaptador de red solo anfitrión ha sido la IP 192.168.1.1. Para acceder a la web de configuración del firewall tendríamos que acceder a: <mark style="color:blue;">`https://192.168.1.1`</mark> pero si lo hago desde mi propio equipo, accedería a la web del router y no al pfsense, con lo cual tendría que hacerlo desde otra VM que estuviera también conectada a la misma red solo anfitrión.&#x20;

Como quiero acceder desde mi propio equipo al pfsense necesitamos que estén en el mismo segmento de red, así que  voy  a realizar una pequeña modificación. Para ello, vamos&#x20;

<figure><img src="../../.gitbook/assets/image (260).png" alt=""><figcaption><p>Adaptadores de red en el equipo anfitrión o host</p></figcaption></figure>

Así que nos vamos a la VM de pfsense y seleccionamos la opción 2 que nos permite configurar los adaptadores de red. Como el adaptador que se quiere modificar es el de la red LAN  volvemos a clicar en la sub-opción 2.

<figure><img src="../../.gitbook/assets/image (262).png" alt=""><figcaption><p>Modificando el adaptador de red para LAN</p></figcaption></figure>

Por tanto, le decimos que la IP será la 192.168.56.2 / 24 y como por ahora no nos interesa configurar el IPv6 respondemos con un no a la configuración del mismo.

A continuación nos pedirá si queremos que el firewall haga de servidor DHCP y si queremos que sea así. Es algo que podemos configurar más adelante pero ya lo podemos tener listo, con lo cual a la pregunta que nos hace de configurar el rango de direcciones IP que queremos que brinde respondemos con un yes.

<figure><img src="../../.gitbook/assets/image (263).png" alt=""><figcaption><p>Configuración del servicio DHCP para la red LAN </p></figcaption></figure>

Le hemos proporcionado el rango de direcciones IP que va desde la 192.168.56.200 a la 192.168.56.210 suficiente para demostrar el funcionamiento del firewall. Confirmamos el cambio y ya lo tenemos como se puede ver en la imagen a continuación:

<figure><img src="../../.gitbook/assets/image (264).png" alt=""><figcaption><p>Modificado la IP del adaptador de red LAN</p></figcaption></figure>

Ahora ya podemos acceder a nuestro pfsense desde el navegador de nuestro equipo escribiendo:



```
http://192.168.56.2
```

Y se nos mostrará el formulario de entrada del firewall, cuyo usuario y contraseña por defecto es: `admin / pfsense`.

<figure><img src="../../.gitbook/assets/image (265).png" alt=""><figcaption><p>Acceso al pfsense</p></figcaption></figure>

Una vez que accedemos vemos la dashboard:

<figure><img src="../../.gitbook/assets/image (266).png" alt=""><figcaption><p>Dashboard de Pfsense</p></figcaption></figure>

### Configuración&#x20;

System > General Setup

Le voy a asignar un nuevo dominio aunque le dejaré el nombre por defecto:

<figure><img src="../../.gitbook/assets/image (267).png" alt=""><figcaption><p>pfsense.kirby.local</p></figcaption></figure>

El servidor de DNS podemos seleccionar que sea: `1.1.1.1` y e `8.8.8.8` y el timezone seleccionamos el que nos corresponde por la ubicación `Europe/Madrid`.

Y como nos aparece un Warning que nos recuerda que el password de admin es el que trae por defecto, vamos a modificarlo:

<figure><img src="../../.gitbook/assets/image (268).png" alt=""><figcaption><p>Modificando el password de admin</p></figcaption></figure>

### Testeando el servicio DHCP del firewall

Para probar el funcionamiento del DHCP podemos levantar una VM que esté en la misma red de solo anfitrión. Un detalle a tener en consideración es que debemos deshabilitar el servicio de DHCP del adaptador de red de Solo Anfitrión en el VirtualBox para asegurar que sea el propio pfsense quien proporcione el servicio.

<figure><img src="../../.gitbook/assets/image (269).png" alt=""><figcaption></figcaption></figure>

### Instalar paquetes&#x20;

Tenemos la posibilidad de instalar algunos paquetes como es el caso de Squid. Los paquetes los podemos ver e instalar desde <mark style="color:blue;">`System`</mark>` ``-` [`Package Manager`](http://192.168.56.2/pkg\_mgr\_installed.php) `-` [`Installed Packages`](http://192.168.56.2/pkg\_mgr\_installed.php)

Así que vamos a instalar este paquete y&#x20;

<figure><img src="../../.gitbook/assets/image (270).png" alt=""><figcaption></figcaption></figure>

Instalamos también el `Squid Guard` y el `openvpn-client-export` paquete que necesitaremos para configurar la VPN en el firewall.

<figure><img src="../../.gitbook/assets/image (271).png" alt=""><figcaption></figcaption></figure>

