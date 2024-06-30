---
description: Con Bind9, en un entorno cliente-servidor
---

# DNS - Ubuntu Server 22.04

A la hora de configurar y administrar una infraestructura de servidores se vuelve importante poder buscar las interfaces de red e IP mediante un nombre. Para ello, podemos configurar un sistema de nombres de dominio (DNS).&#x20;

Utilizar nombres de dominio completos (FQDN), en lugar de direcciones IP para especificar direcciones de red facilita la configuración de servicios y aplicaciones, ayuda a mejorar la administración de los servidores.

Por tanto, aquí veremos cómo configurar un servidor DNS  primario con zonas directa e inversa para la resolución de nombres en nuestra red local. Configuraremos el uso de <mark style="color:blue;">`forwarders`</mark> para las consultas externas y dicho servidor DNS será <mark style="color:blue;">`autoritativo`</mark> para nuestra red.

Para ello utilizaremos el software de servidor de nombres <mark style="color:blue;">`BIND`</mark> (BIND9) en una máquina virtual (MV) <mark style="color:blue;">`Ubuntu 22.04`</mark>.

Sería recomendable tener un segundo servidor Ubuntu 22.04 que sirva como servidor DNS secundario <mark style="color:blue;">NS2</mark>.

### Requisitos previos

* Una MV con Ubuntu Server 22.04&#x20;
* Dos adaptadores de red:
  * NAT o Adaptador Puente
  * Red Interna: 192.168.6.0/24

### Observaciones

Debes tener en cuenta que en esta guía el nombre del servidor de DNS principal que vamos a configurar debería ser <mark style="color:blue;">`ns1`</mark>, pero utilicé una instalación de Ubuntu Server a la que había llamado <mark style="color:red;">`haven`</mark> y no lo modifiqué.

El dominio para el DNS es <mark style="color:blue;">`haven.local`</mark> con lo cual quedaría: <mark style="color:red;">`haven`</mark>.<mark style="color:blue;">`haven.local.`</mark>

Puedesutilizar una <mark style="color:blue;">`redNAT`</mark> en lugar de dos adaptadores (puente o NAT) y la red Interna.

Aunque no lo menciono al inicio, algo que debes tener en cuenta es:&#x20;

* la dirección IP de la red interna o la redNAT que vayas a considerar que, en mi caso es la 192.168.6.100/24 para el servidor.&#x20;
* &#x20;revisar que adaptador(es) de red(es) tienes configurado en tu VM.
* debes tener conexión a Internet desde la VM.
* Configurar una dirección IP estática que si bien no es obligatorio, es recomendable. Lo necesitaremos realmente cuando vayamos a configurar el servicio de DHCP en el mismo servidor.

Algunas razones para configurar una IP estática en un servidor DNS:

**Estabilidad:** Asegura que el servidor DNS siempre se pueda encontrar en la misma dirección. También se utilizan las direcciones dinámicas.

**Rendimiento:** Pueden ser más eficientes en términos de resolución de nombres, ya que no hay necesidad de actualizar registros de DNS cada vez que cambia la dirección IP.

**Seguridad:** Facilita la configuración de reglas en el firewall y políticas de seguridad.

Pudieras utlizar una dirección IP dinámica o DHCP para un servidor DNS pero en nuestro caso, no disponemos de servidor DHCP, todo lo contrario, la idea es instalar el servicio en el mismo servidor para que brinde servicios a varios clientes que se conecten al dominio.

### Actualizar e instalar paquetes&#x20;

Actualizamos los repositorios de nuestro sistema:

```
sudo apt update
```

Una vez hecho esto, procedemos  a instalar el paquete Bind9:

```
sudo apt install bind9
```

#### Configurar

El primer fichero a editar sería&#x20;

<mark style="color:blue;">`/etc/bind/named.conf.local`</mark>&#x20;

Aquí será donde  definiremos las zonas: **directa** e **inversa**, para el dominio **haven.local** haciendo uso de la cláusula **zone**.&#x20;

Cada bloque de zona incluirá de que tipo será la zona y el fichero en el que estarán definidas las características, propiedades y entidades del dominio, para cada zona.

Una práctica adecuada sería hacer una copia de seguridad de los archivos de configuración, como es el caso del archivo <mark style="color:blue;">`named.conf.local`</mark> para lo cual podemos hacer:

```
cp /etc/bind/named.conf.local /etc/bind/named.conf.BKP
```

<figure><img src="../../.gitbook/assets/image (14) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Ahora podemos crear el contenido de cada zona en el fichero de configuración: <mark style="color:blue;">named.conf.local</mark>. Esto es:

<figure><img src="../../.gitbook/assets/image (199).png" alt=""><figcaption><p>/etc/bind/zones/named.conf.local</p></figcaption></figure>

Ahora comprobemos que la configuración es la correcta y no hayamos cometido errores en la sintaxis. Para ello, usamos el comando:&#x20;

```
named-checkconf
```

Si tras lanzar el comando no nos devuelve nada, significa que no se han encontrado errores.

#### **Ficheros de zona**

Ahora vamos a crear los ficheros de zona que hemos indicado en la configuración. Para ello tomemos como plantillas los ficheros de zonas que se crean al instalar el paquete <mark style="color:blue;">bind9</mark> y los editaremos.&#x20;

Para la **zona directa** creamos el fichero:

```
/etc/bind/zones/db.haven.local
```

<figure><img src="../../.gitbook/assets/image (17) (1) (1).png" alt=""><figcaption></figcaption></figure>

Una vez creado dicho fichero ya podemos editarlo y hacer las modificaciones  necesarias para nuestra configuración, como el nombre del dominio.&#x20;

También debemos cambiar la cláusula <mark style="color:blue;">Serial</mark> cada vez que editemos nuestros ficheros de zona, como una forma de llevar un control de versiones.&#x20;

<figure><img src="../../.gitbook/assets/image (108).png" alt=""><figcaption><p>Archivo /etc/bind/db.haven.local </p></figcaption></figure>

Para crear el fichero de <mark style="color:blue;">zona inversa</mark> podemos copiar el fichero db.127:

<figure><img src="../../.gitbook/assets/image (135).png" alt=""><figcaption><p>Fichero de zona inversa copiado de db.127.</p></figcaption></figure>

Editamos el fichero de zona inversa y lo modificaremos, sin olvidarnos del Serial.

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption><p>Fichero de zona inversa /etc/bind/db.6.168.192</p></figcaption></figure>

Al igual que antes, debemos comprobar que la configuración de los ficheros de zonas se ha realizado correctamente y que no hayamos cometido errores. Tener en cuenta que el directorio <mark style="color:blue;">`zones`</mark> que pudieras no haberlo utilizado. Utilizamos el mismo comando:

```
sudo named-checkzone 6.168.192.in-addr-arpa /etc/bind/zones/db.6.168.192
```

seguido del nombre de la zona y del fichero en cuestión. Esto es:

<figure><img src="../../.gitbook/assets/image (148).png" alt=""><figcaption><p>Comprobaciones</p></figcaption></figure>

```
sudo named-checkzone haven.local /etc/bind/zones/db.haven.local
```

<figure><img src="../../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

Ahora nos tocaría editar el fichero <mark style="color:blue;">`/etc/bind/named.conf.options`</mark> donde podemos crear una <mark style="color:blue;">`lista de acceso`</mark> para restringir el acceso a quienes pueden realizar las consultas a nuestro servidor DNS. También indicaremos un par de <mark style="color:blue;">`servidores forwarders`</mark> donde pueda delegar nuestro servidor DNS local cuando no pueda resolver alguna consulta.

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Fichero /etc/bind/named.conf.options</p></figcaption></figure>

Ya casi finalizamos, pero antes de poner en marcha el servicio modifiquemos  el fichero <mark style="color:blue;">`/etc/default/named`</mark> donde especificaremos la opción<mark style="color:blue;">`-4`</mark> como argumento para el usuario **bind,** que  se crea automáticamente durante la instalación del servicio bind9.&#x20;

La opción <mark style="color:blue;">`-4`</mark>  nos sirve para forzar el uso de IPv4 siempre y evitar  mensajes de error de red inalcanzable por direccionamiento IPv6.

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption><p>Fichero /etc/default/named</p></figcaption></figure>

Listo, ya tenemos configurado el servidor DNS. Solo nos queda <mark style="color:blue;">`reiniciar el servicio bind9`</mark> y comprobar que esté corriendo correctamente.

<figure><img src="../../.gitbook/assets/image (20) (1) (1).png" alt=""><figcaption></figcaption></figure>

Si no las hecho antes, **edita la configuración de red** del servidor de Ubuntu  para indicar que él mismo es el servidor DNS que tendrá que consultar para la resolución de nombres. Y esto lo tenemos que hacer con **netplan**.

<figure><img src="../../.gitbook/assets/image (47) (1).png" alt=""><figcaption><p>Configuración de la red con netplan</p></figcaption></figure>

Para que tome los cambios, hacemos:

```
sudo netplan apply
```

Ya podemos  realizar pruebas con **nslookup** para comprobar si el servidor DNS está resolviendo correctamente los nombres y las direcciones IP:

<figure><img src="../../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>

Nos sigue respondiendo con una respuesta no autoritativa pese a que no debería ser así.&#x20;

Las aplicaciones hacen uso del resolver de systemd-resolved  escuchan en la interfaz de loopback en la dirección 127.0.0.53. Por lo cual las respuestas a nuestras consultas son no autoritativas, ya que nos está respondiendo el resolver de systemd-resolved que a su vez obtiene la respuesta de nuestro resolver (bind9).

&#x20;El archivo **/etc/resolv.conf** es un enlace simbólico a otro fichero de configuración. Por cada modo de implementación del systemd-resolved, existen **4 ficheros de configuración.**

<figure><img src="../../.gitbook/assets/image (53) (1).png" alt=""><figcaption><p>En este caso /etc/resolv.conf ya apunta al fichero adecuado</p></figcaption></figure>

¿Cómo podemos desactivar el systemd-resolved y que las aplicaciones consulten nuestro resolver directamente?&#x20;

Si haces:

```
ls -l /etc/resolv.conf
```

verás que apunta a un archivo en:

```
/run/systemd/resolve/stub-resolv.conf
```

Entonces, modificamos el enlace simbólico haciendo lo siguiente:

```
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

para hacerlo apuntar al fichero adecuado. Comprueba haciendo:

```
cat /etc/resolv.conf
```

<figure><img src="../../.gitbook/assets/image (61) (1).png" alt=""><figcaption></figcaption></figure>

Si vuelves a probar el nslookup veremos que funciona como se espera que haga:

<figure><img src="../../.gitbook/assets/image (13) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Links

* [https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04-es](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04-es)
* [https://www.freedesktop.org/software/systemd/man/systemd-resolved.service.html#/etc/resolv.conf](https://www.freedesktop.org/software/systemd/man/systemd-resolved.service.html#/etc/resolv.conf)
* [https://netplan.io/examples](https://netplan.io/examples)
* [https://blog.sysdual.com/instalacion-y-configuracion-del-servicio-dns-en-ubuntu-server-20-04/](https://blog.sysdual.com/instalacion-y-configuracion-del-servicio-dns-en-ubuntu-server-20-04/)&#x20;

