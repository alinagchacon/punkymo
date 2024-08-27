---
description: Configurando una DMZ
---

# DMZ

## DMZ -  Zona Desmilitarizada

Se trata de una red que se encuentra entre la red interna de una organización e Internet (red no confiable). En una configuración típica la DMZ ofrece servicios y recursos que necesitan ser accesibles desde Internet, como puede ser: servicio web, servicio de correo electrónico, etc. Estos servicios de la DMZ están expuestos a amenazas, pero se encuentran fuera de la red interna, separadas por uno o más firewalls, según el diseño de la red establecido.

La configuración de la DMZ se establece de modo que el tráfico desde y hacia Internet pueda acceder a los servicios en la DMZ, pero evitando el acceso a la red interna de la organización. Esto se consigue mediante reglas de firewall y configuraciones de enrutamiento que permiten el control  del tráfico de datos entre las diferentes zonas de la red.

### Nuestro escenario

La idea es probar el tráfico a través del firewall, permitiendo el acceso a dos servicios instalados en dos equipos de la DMZ.

Para nuestra prueba voy a utilizar mis dos VM con Debian:&#x20;

* En una VM el servicio **web básico HTTP** con Nginx
* En otra VM el servicio **SSH** con la configuración por defecto

Al firewall pfSense le vamos habilitar tres interfaces de red:&#x20;

* **Bridge** para la salida a Internet como WAN
* **Solo anfitrión** para la LAN
* **Red interna** para la DMZ

En mi caso, queda algo como lo siguiente:

| Adaptador      | Red                             |
| -------------- | ------------------------------- |
| Puente         | 192.168.1.53 /24 \| DHCP        |
| Solo anfitrión | 192.168.56.2 /24 \| IP estática |
| Red Interna    | 172.27.1.2 /24 \| IP estática   |

En mi pfSense se puede ver de la siguiente manera:

<figure><img src="../../../../.gitbook/assets/image (8) (1) (1) (1) (1).png" alt=""><figcaption><p>Configuración del pfSense</p></figcaption></figure>

En el diagrama de la red se muestran las dos VM conectadas a la DMZ, así como los servidores y el servicio que van a brindar: HTTP y SSH.

<figure><img src="../../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Diagrama de la red</p></figcaption></figure>

### La interfaz de red de la DMZ

Para habilitar la interfaz de red para la DMZ necesitas primero tener apagada la VM del pfSense, agregar el tercer adaptador de red en modo `red interna` y encender el firewall. Adicionalmente, debemos renombrar la interfaz como DMZ dado que el sistema la nombra como `Option 1` por defecto.

<figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Las tres interfaces de red</p></figcaption></figure>

Las direcciones IP de las interfaces de red LAN y DMZ son IP estáticas y en las mismas tenemos que configurar el servicio DHCP para que le brinde direcciones IP a las VM que vamos a conectar por cada interfaz de red, esto es:

| Interfaz | IP de inicio   | IP final       |
| -------- | -------------- | -------------- |
| LAN      | 192.168.56.200 | 192.168.56.210 |
| DMZ      | 172.27.1.200   | 172.27.1.210   |

La siguiente imagen muestra las asignaciones de las IP a las VM conectadas por cada interfaz de red: LAN y DMZ.

<figure><img src="../../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Las IP asignadas a las VM conectadas a las redes LAN y DMZ</p></figcaption></figure>



## Reglas de salida (outbond) y reglas de entrada (port forward)

Antes de continuar con nuestra configuración de los servicios en la DMZ, primero aclaremos algo.&#x20;

De modo automático, pfSense genera `reglas de salida` a través de la `WAN` para las redes que tenemos declaradas en cada interfaz del firewall. Dichas reglas se pueden generar de manera manual también y las encontramos en: `Firewall - NAT - Outbond`.&#x20;

Dado que tenemos declaradas las redes `LAN` y `DMZ` encontraremos dos reglas por defecto.

<figure><img src="../../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Reglas de salida en el firewall</p></figcaption></figure>

Si queremos crear `reglas de entrada` en el pfSense tenemos que ir a: `Firewall - NAT - Port Forward`. Por tanto, esto es lo que vamos a hacer para cada servicio: el de HTTP y el de SSH.

## Acceso al servicio web - 172.27.1.200

El primer servicio que vamos a habilitar será la web  que se aloja en el servidor con IP 172.27.1.200. Es un servicio básico a través del puerto 80.&#x20;

### Crear la regla de entrada

Vamos a crear una regla de mapeo que permita que todo lo que venga a través de la WAN hará la traducción de red (recuerda que es una regla NAT). A su vez, queremos que reenvíe el tráfico hacia el puerto 80 del servidor web que tenemos en la VM que está en la DMZ. Para ello, nos dirigimos a `Firewall - NAT - Port Forward` y añadimos una nueva regla.

Las opciones a tener en cuenta en la nueva regla NAT a crear son:

* Interfaz: WAN
* Address family: IPv4
* Protocol: TCP
* Destination: WAN address
* Destination port: <mark style="color:red;">HTTP</mark> (puerto 80 por defecto)
* Redirect target IP: single host - 172.27.1.200&#x20;
* Redirect target port: HTTP (puerto 80 por defecto)
* Description: regla NAT en WAN

Una vez guardados los cambios, se nos visualizará como sigue:

<figure><img src="../../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>La regla NAT permitiendo el acceso al puerto 80 de HTTP en el server de la DMZ</p></figcaption></figure>

Esta misma regla también la podemos encontrar en `Firewall - Rules - WAN`.

### Comprobando la conexión

Una vez creada la regla NAT podemos comprobar que visualizamos nuestro sitio web a través del firewall.&#x20;

¿Qué IP tenemos que poner en el navegador de nuestro equipo anfitrión para llegar al sitio web que tenemos en la DMZ? Efectivamente, la IP pública del firewall que, en mi caso, sería: `192.168.1.53` (cambié de pfSense y dejé la IP que me asignó por defecto: 192.168.1.106). Así que cuando hacemos la solicitud de acceso a la página web del servidor que se encuentra en la DMZ el firewall nos redirige y nos muestra la web:

<figure><img src="../../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>La pàgina web visible desde la WAN</p></figcaption></figure>

## Acceso al servicio SSH - 172.27.1.201

En esta ocasión activaremos el acceso al servicio de SSH que se encuentra en otro servidor en la DMZ. El servicio está en otra VM con Debian con la IP 172.27.1.201 y el puerto 22 por defecto.

Volvemos a Firewall - `NAT - Port Forward` y clicamos en el botón de añadir una nueva regla NAT. Las opciones a tener en cuenta en esta ocasión son similares a la anterior:

* Interfaz: WAN
* Address family: IPv4
* Protocol: TCP
* Destination: WAN address
* Destination port: <mark style="color:red;">SSH</mark> (puerto 22 por defecto)
* Redirect target IP: single host - 172.27.7.201
* Redirect target port: SSH (puerto 22 por defecto)
* Description: regla NAT en WAN para SSH en DMZ

Una vez guardados los cambios, se nos visualizará junto a la primera regla NAT que hemos creado para el protocolo HTTP:

<figure><img src="../../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Reglas NAT para HTTP y SSH </p></figcaption></figure>

### Comprobando el servicio de SSH

Para testear el servicio de SSH me he ido al terminal y he tecleado la IP pública del pfsense:

```
ssh -p 22 user@192.168.1.53 
```

y puedo comprobar que me permite el acceso al dispositivo donde tengo habilitado el servicio SSH.

Recuerda que la seguridad a nivel de red es en capas como una cebolla. No basta configurar el firewall permitiendo el tráfico hacia determinados servicios y puertos. También es necesario disponer de firewall a nivel local: Windows Defender, IPTables, UFW para Ubuntu, a modo de otra capa extra de protección.

## Otras opciones&#x20;

Para probar el firewall podemos  utilizar:&#x20;

* Una de las VM donde hemos instalado y configurado el `servicio web` con nginx con un dominio y puertos diferentes al puerto 80. Por ejemplo: www.punky.com:8080&#x20;
* El `servidor de correos`
* Una VM con el servicio de `FTP` instalado.

