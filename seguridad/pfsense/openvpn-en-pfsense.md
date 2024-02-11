# OpenVPN en Pfsense

A la hora de configurar una VPN necesitamos recorrer los siguientes pasos:

1. Instalar el plugin OpenVPN Client para generar la configuración&#x20;
2. Crear los certificados digitales en el propio pfSense&#x20;
   1. Crear la CA (Autoridad de Certificación)&#x20;
   2. Crear el certificado del servidor OpenVPN
   3. Crear los certificados de todos los clientes&#x20;
3. Configurar el servidor OpenVPN con todas las opciones explicadas&#x20;
4. Configurar las reglas en el firewall para permitir acceso&#x20;
5. Exportar el archivo de configuración OpenVPN para los clientes&#x20;
6.  Comprobar estado del servicio y de los clientes conectados



## Instalar el plugin OpenVPN client

Lo primero es descargar el paquete `openvpn-client-export` y para ello vamos a _`System - Package Manager - Available Packages`_ y buscamos el paquete _`openvpn-client-export`_ y pulsamos en `Install`_._

<figure><img src="../../.gitbook/assets/image (274).png" alt=""><figcaption><p>Instalación finalizada del paquete pfsense-client-export</p></figcaption></figure>

## Crear los certificados digitales en el propio pfSense&#x20;

### Crear la CA (Autoridad de Certificación)&#x20;

Una `CA` es, una entidad confiable responsable de emitir y revocar certificados digitales utilizados para transacciones y firmas electrónicas.\
Abrimos la interfaz del pfSense y navegamos hasta `System - Certificate Manager` y hacemos clic en `Agregar`.

<figure><img src="../../.gitbook/assets/image (272).png" alt=""><figcaption></figcaption></figure>

Mantenemos casi todas las opciones por defecto asignando un nombre y creando el certificado

* Descriptive name: OpenVPN\_CA
* Common name: OpenVPN\_CA
* Creamos un certificado (opciones por defecto)

<figure><img src="../../.gitbook/assets/image (275).png" alt=""><figcaption></figcaption></figure>

### Crear los certificados del servidor OpenVPN

En el apartado siguiente, `System – Certificates` clicamos en agregar un certificado:

* Method: Create an Internal Certificates
* Descriptive name: OpenVPN\_Certificates
* Common Name: OpenVPN\_Certificates
* Certificate type: Server certificate&#x20;

Aquí nos tiene que aparecer nuestra CA previamente creada

<figure><img src="../../.gitbook/assets/image (276).png" alt=""><figcaption><p>Open VPN server certificate</p></figcaption></figure>



### Crear los certificados de todos los clientes

?????

### &#x20;Configurar el servidor OpenVPN

Ahora vamos a configurar el servidor OpenVPN a donde se van a conectar los clientes para lo cual, nos vamos a <mark style="color:blue;">`VPN`</mark>` ``-` [`OpenVPN`](http://192.168.56.2/vpn\_openvpn\_server.php) `-` [`Servers`](http://192.168.56.2/vpn\_openvpn\_server.php) `y` clicamos en `Add` y rellenamos las opciones:

* Description: OPENVPN\_Server
* Server mode: Remote Access (SSL/TLS + User Auth)&#x20;
* Protocol: UDP on IPv4 only&#x20;
* Interface: WAN
* Puerto: 5194 (cambiamos el puerto por defecto)
* Peer Certificate Authority: OpenVPN\_CA (seleccionamos el nuestro)
* Server certificate: OPENVPN\_Certificate (Server: Yes, CA: OPENVPN\_CA)&#x20;

<figure><img src="../../.gitbook/assets/image (277).png" alt=""><figcaption></figcaption></figure>

* Por último, debemos seleccionar la red que vamos a utilizar para comunicar el cliente con el servidor (pfsense). En nuestro caso podemos seleccionar la red: 10.4.44.0/24, así como las redes o IP internas a las que tendrá acceso el cliente cuando se conecte por la VPN.&#x20;
* Hacemos clic en `Redirect IPv4 Gateway: Force all client-generated IPv4 traffic through the tunnel`. para seleccionar esta opción
* Podemos seleccionar la opción `Inter-client communication` para permitir comunicación entre clientes de la VPN.
* Podemos seleccionar la opción `Duplicate connection` para permitir varias conexiones de un mismo cliente.

<figure><img src="../../.gitbook/assets/image (278).png" alt=""><figcaption><p>Algunas de las opciones a seleccionar </p></figcaption></figure>

* Podemos proporcionar el dominio a los clientes y los dns a los que poder acceder.
  * DNS Server 1: 1.1.1.1
  * DNS Server 2: 8.8.8.8
* Seleccionamos la opción de `Verbosity level: 3 (recommended)`.&#x20;
* Guardamos y listo!



