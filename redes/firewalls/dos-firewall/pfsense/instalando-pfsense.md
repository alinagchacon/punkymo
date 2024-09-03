---
description: Instalación básica
---

# Instalando pfSense

### ¿Dónde lo descargo?

Para descargar la ISO debemos ir a la web oficial de pfSense en:

<pre><code><strong>https://www.pfsense.org/download/
</strong></code></pre>

Y seleccionar las opciones correspondientes:

<figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="366"><figcaption><p>Download pfSense</p></figcaption></figure>

### Instalación&#x20;

Vamos hacer la instalación en una VM en VirtualBox. Utilizaremos dos adaptadores de red:

1. Adaptador de red: Bridge
2. Adaptador de red: solo Anfitrión

Para instalar pfSense en la MV voy a utilizar la siguiente configuración:

* RAM:  2048
* HDD:  16 GB
* S.O.:  BSD

Instalamos todo por defecto. Apagamos. Quitamos la ISO y reiniciamos. Una vez que nos sale la pantalla inicial, comenzamos a configurar.

<figure><img src="../../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>En este pantallazo ya configuré los adaptadores de red</p></figcaption></figure>

En el menú de pfSense podemos ver cada una de las opciones que nos brinda y que podemos configurar, aunque lo adecuado es acceder  a la web para configurar los servicios.

<table><thead><tr><th width="234">Adaptador / Servicio</th><th>IP / rango IP</th></tr></thead><tbody><tr><td>WAN -> em0</td><td>192.168.1.53/24 (por DHCP)</td></tr><tr><td>LAN -> em1</td><td>192.168.56.2/24 (IP estática)</td></tr><tr><td>Servicio DHCP </td><td>FROM 192.168.56.100 TO 192.168.56.105</td></tr></tbody></table>

### Diagrama de red

Vamos a configurar un escenario muy básico como el que se muestra a continuación. El "problema" que podemos tener para probarlo es que estaremos detrás de un router al que no tenemos acceso a menos que lo configuremos en casa.&#x20;

Según la documentación publicada en [https://docs.netgate.com/pfsense/en/latest/recipes/rfc1918-egress.html](https://docs.netgate.com/pfsense/en/latest/recipes/rfc1918-egress.html) y el documento `RFC 1918` publicado por la `Internet Engineering Task Force (IETF)` titulado "**Address Allocation for Private Internets**", las direcciones `RFC 1918` son bloques de direcciones IP de red reservadas para uso privado. Estas direcciones se usan comúnmente detrás de firewalls para permitir que una única dirección IP pública se comparta con múltiples dispositivos mediante NAT. La instalación predeterminada del software pfSense® asigna el espacio de direcciones 192.168.1.0/24 a la interfaz LAN, pero RFC 1918 también define otros rangos CIDR para uso privado:

* 10.0.0.0/8
* 172.16.0.0/12
* 192.168.0.0/16

Como regla general, es una buena práctica evitar que el tráfico de red destinado a las subredes `RFC 1918` salga del firewall a través de la interfaz `WAN`. Esto evita el tráfico innecesario en el enlace `WAN` y también proporciona unos **mínimos de seguridad** al mantener la información sobre la red LAN detrás del firewall.

<figure><img src="../../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Configuración de la red</p></figcaption></figure>

### Configuración básica

Una vez instalado el pfSense, la IP que nos ha asignado para el adaptador de red solo anfitrión ha sido la IP 192.168.1.1. Para acceder a la web de configuración del firewall tendríamos que acceder a: <mark style="color:blue;">`https://192.168.1.1`</mark> pero si lo hago desde mi propio equipo, accedería a la web del router y no al pfSense, con lo cual tendría que hacerlo desde otra VM que estuviera también conectada a la misma red solo anfitrión. &#x20;

Como quiero acceder desde mi propio equipo al pfSense necesitamos que estén en el mismo segmento de red, así que  voy  a realizar una pequeña modificación. Podemos observar los adaptadores de red que tengo en mi equipo y la que nos interesa es el `vboxnet0: 192.168.56.1/24`.

<figure><img src="../../../../.gitbook/assets/image (260).png" alt=""><figcaption><p>Adaptadores de red en el equipo anfitrión o host</p></figcaption></figure>

Así que nos vamos a la VM de pfSense y seleccionamos la opción **2** que nos permite configurar los adaptadores de red. Como el adaptador que se quiere modificar es el de la red LAN  volvemos a clicar en la sub-opción **2**.

<figure><img src="../../../../.gitbook/assets/image (262).png" alt=""><figcaption><p>Modificando el adaptador de red para LAN</p></figcaption></figure>

Por tanto, le decimos que la IP será la `192.168.56.2 / 24` y como por ahora no nos interesa configurar el IPv6 respondemos con un no a la configuración del mismo.

A continuación nos pedirá si queremos que el firewall haga de `servidor DHCP` y si queremos que sea así. Es algo que podemos configurar más adelante pero ya lo podemos tener listo, con lo cual a la pregunta que nos hace de configurar el rango de direcciones IP que queremos que brinde respondemos con un `yes`.

<figure><img src="../../../../.gitbook/assets/image (263).png" alt=""><figcaption><p>Configuración del servicio DHCP para la red LAN </p></figcaption></figure>

Le hemos proporcionado el rango de direcciones IP que va desde la `192.168.56.200` a la `192.168.56.210` suficiente para demostrar el funcionamiento del firewall. Confirmamos el cambio y ya lo tenemos como se puede ver en la imagen a continuación:

<figure><img src="../../../../.gitbook/assets/image (264).png" alt=""><figcaption><p>Modificado la IP del adaptador de red LAN</p></figcaption></figure>

Ahora ya podemos acceder a nuestro pfSense desde el navegador de nuestro equipo escribiendo:

```
http://192.168.56.2
```

Y se nos mostrará el formulario de entrada del firewall, cuyo usuario y contraseña por defecto es: `admin / pfsense`.

<figure><img src="../../../../.gitbook/assets/image (265).png" alt=""><figcaption><p>Acceso al pfsense</p></figcaption></figure>

Una vez que accedemos podemos ver el dashboard y analizar la información que nos brinda.

<figure><img src="../../../../.gitbook/assets/image (266).png" alt=""><figcaption><p>Dashboard de Pfsense</p></figcaption></figure>

### Configuración&#x20;

Comenzamos con unos mínimos de configuración del sistema. Para ello nos vamos a:

System > General Setup

Le voy a asignar un nuevo dominio aunque le dejaré el nombre por defecto:

<figure><img src="../../../../.gitbook/assets/image (267).png" alt=""><figcaption><p>pfsense.kirby.local</p></figcaption></figure>

El servidor de DNS podemos seleccionar que sea: `1.1.1.1` y e `8.8.8.8` y el timezone seleccionamos el que nos corresponde por la ubicación `Europe/Madrid`.

Y como nos aparece un `Warning` que nos recuerda que el password de admin es el que trae por defecto, vamos a modificarlo:

<figure><img src="../../../../.gitbook/assets/image (268).png" alt=""><figcaption><p>Modificando el password de admin</p></figcaption></figure>

### Testeando el servicio DHCP del firewall

Para probar el funcionamiento del servicio de DHCP podemos levantar una VM que esté en la misma red de solo anfitrión. Un detalle a tener en consideración es que debemos **deshabilitar** el servicio de `DHCP` del adaptador de red de `Solo Anfitrión` en el `VirtualBox` para asegurar que sea el propio pfSense quien proporcione el servicio.

<figure><img src="../../../../.gitbook/assets/image (269).png" alt=""><figcaption><p>Deshabilitar el servicio de DHCP para el adaptador solo anfitrión en VirtualBox</p></figcaption></figure>

Encendemos una VM conectada al adaptador solo anfitrión (en mi caso es un debian) y como se puede observar, tiene la IP 192.168.56.200 brindada por el pfSense.

<figure><img src="../../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>VM cliente conectada al firewall pfSense</p></figcaption></figure>

### Instalar otros paquetes&#x20;

Tenemos la posibilidad de instalar algunos paquetes como es el caso de `Squid` que lo configuraremos más adelante. Los paquetes los podemos ver e instalar desde <mark style="color:blue;">`System`</mark>` ``-` [`Package Manager`](http://192.168.56.2/pkg\_mgr\_installed.php) `-` [`Installed Packages`](http://192.168.56.2/pkg\_mgr\_installed.php)

Así que vamos a instalar los paquetes `Squid`, `SquidGuard` y el `openvvpn-client-export` que es el que realmente vamos a necesitar en breve.

<figure><img src="../../../../.gitbook/assets/image (270).png" alt=""><figcaption><p>Paquetes de Squid a instalar en pfSense</p></figcaption></figure>

Instalamos también el `Squid Guard` y el `openvpn-client-export` paquete que necesitaremos para configurar la VPN en el firewall.

<figure><img src="../../../../.gitbook/assets/image (271).png" alt=""><figcaption><p>Paquete de openvpn-client-export a instalar en pfSense</p></figcaption></figure>

Ahora lo dejamos en este punto y continuamos con la instalación y configuración de una `VPN` en `pfSense`.
