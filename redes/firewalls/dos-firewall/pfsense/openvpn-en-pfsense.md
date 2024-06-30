# OpenVPN en pfSense

### ¿Qué es un servidor VPN?

Se trata de una herramienta  importante dentro del ámbito de la seguridad y la privacidad en línea. Un servidor VPN nos facilita una conexión segura y cifrada entre un dispositivo cliente (PC, smartphone) y una red privada a través de Internet. Por tanto, su función principal es establecer un `túnel seguro que cifra el tráfico de datos entre el cliente y el servidor`, proporcionando privacidad y seguridad en la comunicación.

Al conectar un dispositivo a un servidor VPN, todo el tráfico se enruta a través de este túnel, haciendo que la información que se transmite sea más segura.&#x20;

La función principal de un servidor VPN es la de crear una capa de privacidad y seguridad al enrutar el tráfico a través de una conexión cifrada. Cuestión especialmente importante al utilizar redes públicas, como las de bares, aeropuertos o cualquier otro sitio con acceso WiFi, donde los datos pueden ser vulnerables a ataques cibernéticos.

Por otra parte, un servidor VPN permite a los usuarios acceder a una red privada de manera remota, permitiendo el acceso a recursos como archivos compartidos  o aplicaciones internas.

### Protocolos

Existen diferentes tipos de protocolos que pueden ser utilizados por los servidores VPN para establecer la conexión segura. Algunos son:

* OpenVPN
* IPSec
* L2TP/IPSec&#x20;

También conocemos de la existencia de servicios VPN gestionados por terceros que ofrecen acceso a servidores VPN en ubicaciones geográficas diversas, permitiendo a los usuarios eludir restricciones geográficas y proteger su privacidad en línea.&#x20;

### OpenVPN

Es una implementación de código abierto del protocolo VPN que nos permite establecer conexiones seguras a través de Internet.&#x20;

Cuando se menciona la "OpenVPN integrada en pfSense", se refiere a la capacidad de pfSense para actuar como un servidor OpenVPN. Con esta integración, pfSense permite a los usuarios configurar y administrar conexiones VPN utilizando el protocolo OpenVPN directamente desde la interfaz de administración web de pfSense.

Al utilizar OpenVPN en pfSense, los usuarios pueden aprovechar características como la autenticación de usuarios, la encriptación del tráfico y la creación de túneles seguros para el acceso remoto a la red o la interconexión segura de redes a través de Internet. La integración de OpenVPN en pfSense facilita la implementación de soluciones VPN para garantizar la privacidad y seguridad de las comunicaciones en la red.

### ¿Qué necesitamos para configurar un servicio de VPN en pfSense?

A la hora de configurar una VPN necesitamos recorrer los siguientes pasos:

1. Instalar el plugin OpenVPN Client para generar la configuración&#x20;
2. Crear los certificados digitales en el propio pfSense&#x20;
   1. Crear la CA (Autoridad de Certificación)&#x20;
   2. Crear el certificado del servidor OpenVPN
3. Configurar el servidor OpenVPN&#x20;
4. Configurar las reglas en el firewall para permitir acceso&#x20;
5. Exportar el archivo de configuración OpenVPN para los clientes&#x20;
6. Comprobar estado del servicio y de los clientes conectados

## Instalar el plugin OpenVPN client

Lo primero es descargar el paquete `openvpn-client-export` y para ello vamos a _`System - Package Manager - Available Packages`_ y buscamos el paquete _`openvpn-client-export`_ y pulsamos en `Install`_._

<figure><img src="../../../../.gitbook/assets/image (274).png" alt=""><figcaption><p>Instalación finalizada del paquete pfsense-client-export</p></figcaption></figure>

## Crear los certificados digitales en el propio pfSense&#x20;

### Crear la CA (Autoridad de Certificación)&#x20;

Una `CA` es, una entidad confiable responsable de emitir y revocar certificados digitales utilizados para transacciones y firmas electrónicas.\
Abrimos la interfaz del pfSense y navegamos hasta `System - Certificate Manager` y hacemos clic en `Agregar`.

<figure><img src="../../../../.gitbook/assets/image (272).png" alt=""><figcaption></figcaption></figure>

Creamos el certificado manteniendo casi todas las opciones por defecto y asignando un nombre.

<table><thead><tr><th width="312">Opción</th><th>Descripción</th></tr></thead><tbody><tr><td>Descriptive name</td><td>OpenVPN_CA</td></tr><tr><td>Common name</td><td>OpenVPN_CA</td></tr><tr><td></td><td></td></tr></tbody></table>

Una vez realizado los cambios y guardando nos aparece nuestro certificado:

<figure><img src="../../../../.gitbook/assets/image (275).png" alt=""><figcaption><p>Certificado de openvpn</p></figcaption></figure>

### Crear el certificado del servidor OpenVPN

En el apartado siguiente, `System – Certificates` clicamos en agregar un certificado:

| Opción           | Descripción                      |
| ---------------- | -------------------------------- |
| Method           |  Create an Internal Certificates |
| Descriptive name | OpenVPN\_Certificates            |
| Common Name      | OpenVPN\_Certificates            |
| Certificate type | Server certificate               |

Aquí nos tiene que aparecer nuestra CA previamente creada

<figure><img src="../../../../.gitbook/assets/image (276).png" alt=""><figcaption><p>Open VPN server certificate</p></figcaption></figure>

### Configurar el servidor OpenVPN

Ahora vamos a configurar el `servidor` `OpenVPN` a donde se van a conectar los clientes para lo cual, nos vamos a <mark style="color:blue;">`VPN`</mark>` ``-` [`OpenVPN`](http://192.168.56.2/vpn\_openvpn\_server.php) `-` [`Servers`](http://192.168.56.2/vpn\_openvpn\_server.php) `y` clicamos en `Add` y rellenamos las opciones:



<table><thead><tr><th width="284">Opción</th><th>Descripción</th></tr></thead><tbody><tr><td>Description</td><td>OPENVPN_Server</td></tr><tr><td>Server mode</td><td>Remote Access (SSL/TLS + User Auth) </td></tr><tr><td>Protocol</td><td>UDP on IPv4 only </td></tr><tr><td>Interface</td><td>WAN</td></tr><tr><td>Puerto</td><td>5194 (cambiamos el puerto por defecto)</td></tr><tr><td>Peer Certificate Authority</td><td> OpenVPN_CA (seleccionamos el nuestro)</td></tr><tr><td>Server certificate</td><td>OPENVPN_Certificate (Server: Yes, CA: OPENVPN_CA) </td></tr></tbody></table>

<figure><img src="../../../../.gitbook/assets/image (277).png" alt=""><figcaption><p>NUestro certificado de openVPN</p></figcaption></figure>

* Por último, debemos seleccionar la red que vamos a utilizar para comunicar el cliente con el pfSense. En mi caso voy a seleccionar la red: `10.4.44.0/24`, así como las redes o IP internas a las que tendrá acceso el cliente cuando se conecte por la VPN.&#x20;
* Hacemos clic en `Redirect IPv4 Gateway: Force all client-generated IPv4 traffic through the tunnel`. para seleccionar esta opción
* Podemos seleccionar la opción `Inter-client communication` para permitir comunicación entre clientes de la VPN.
* Podemos seleccionar la opción `Duplicate connection` para permitir varias conexiones de un mismo cliente.

<figure><img src="../../../../.gitbook/assets/image (278).png" alt=""><figcaption><p>Algunas de las opciones a seleccionar </p></figcaption></figure>

* Podemos proporcionar el dominio a los clientes y los DNS a los que poder acceder.
  * DNS Server 1: 1.1.1.1
  * DNS Server 2: 8.8.8.8
* Seleccionamos la opción de `Verbosity level: 3 (recommended)`.&#x20;
* Guardamos y listo!

<figure><img src="../../../../.gitbook/assets/image (9) (1) (1) (1) (1).png" alt=""><figcaption><p>OpenVPN certificate</p></figcaption></figure>

### Comprobar el servicio

Nos dirigimos a `Status - Service` y podemos comprobar los servicios activos y recién habilitados.

<figure><img src="../../../../.gitbook/assets/image (10) (1) (1) (1) (1).png" alt=""><figcaption><p>Status - Service</p></figcaption></figure>

### Configurar las reglas en el firewall para permitir acceso&#x20;

Ahora nos toca crear una regla en la WAN que nos permita el acceso a través del puerto de VPN. &#x20;

#### Regla en "Firewall - Rules - WAN"

Para ello, clicamos en `Firewall - Rules - WAN`  y vamos a crear la regla, clicando donde dice `Add rule to the top of the list.`

<figure><img src="../../../../.gitbook/assets/image (11) (1).png" alt=""><figcaption><p>Crear la regla en firewall - rules - WAN</p></figcaption></figure>

Seleccionamos las opciones siguientes, para una configuración básica:



| Opción                 | Descripción                        |
| ---------------------- | ---------------------------------- |
| Action                 | Pass                               |
| Interface              | WAN                                |
| Protocol               | UDP                                |
| Source                 | Any                                |
| Destination            | Any                                |
| Destination port range | 5194                               |
| Logs                   | Seleccionamos la opción de guardar |
| Description            | OPENVPN:RULE                       |

Una vez realizados los cambios, guardamos y se nos muestra como sigue:

<figure><img src="../../../../.gitbook/assets/image (13).png" alt=""><figcaption><p>Regla en WAN para permitir el tráfico VPN </p></figcaption></figure>

#### Regla para permitir todo el tráfico VPN

Ahora nos vamos a la pestaña de `OpenVPN` para crear otra regla que permita todo el tráfico. Seleccionamos todos los protocolos (ANY) y desde cualquier origen (ANY) a cualquier destino (ANY).

<figure><img src="../../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Seleccionamos también la opción de almacenar los logs del tráfico. Guardamos y se nos crea la regla.

<figure><img src="../../../../.gitbook/assets/image (15).png" alt=""><figcaption><p>Regla para permitir todo el tráfico en la OpenVPN</p></figcaption></figure>

### Exportar el archivo de configuración OpenVPN para los clientes&#x20;

#### Crear un cliente para la VPN

Primero tenemos que crear un usuario nuevo, por tanto nos vamos a `System - User Manager`&#x20;

<figure><img src="../../../../.gitbook/assets/image (16).png" alt=""><figcaption><p>Cerar un usuario </p></figcaption></figure>

Creamos un nuevo usuario y clicamos en `Click to create a user certificate` para crear el certificado para ese usuario.

<figure><img src="../../../../.gitbook/assets/image (17).png" alt=""><figcaption><p>Usuario Punky creado</p></figcaption></figure>

Una vez creado nuestro usuario de prueba (con su certificado para la VPN) ver cómo  exportar los clientes VPN. Para ello seleccionamos en el menú  a `VPN - OpenVPN - Client Export.`

Y si nos vamos al final del todo, veremos a nuestro usuario **Punky**.

<figure><img src="../../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Certificados a exportar del usuario</p></figcaption></figure>

### Comprobar estado del servicio y de los clientes conectados

Si queremos hacer una prueba  podemos descargar el certificado para Android o el de OpenVPN Connect (IOS/Android).

En el caso de utilizar un dispositivo móvil, tendríamos que instalar la `OpenVPN for Android`&#x20;

<figure><img src="../../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="48"><figcaption><p>OpenVPN for Android</p></figcaption></figure>

Una vez tengamos lista la app, podemos exportar a nuestro dispositivo móvil el certificado del cliente VPN que hayamos creado. En mi caso, sería el usuario `punky`

<figure><img src="../../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Certificado de usuario Importado en la aplicación OpenVPN</p></figcaption></figure>

Nos conectamos a la VPN y se nos muestra el log:

<figure><img src="../../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Usuario conectado a la VPN de pfSense </p></figcaption></figure>

Y si nos vamos, por ejemplo al navegador y escribimos la IP

<figure><img src="../../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Accediendo a pfSense en el navegador</p></figcaption></figure>

Ya podemos comprobar la conexión al firewall.  Claramente que nos falta mucho. Se trata de una conexión básica. Lo siguiente a probar es configurar el acceso de un usuario a un equipo de la red detrás del firewall.&#x20;



### Links

[https://www.cloudflare.com/es-es/learning/network-layer/what-is-ipsec/](https://www.cloudflare.com/es-es/learning/network-layer/what-is-ipsec/)

[https://www.pfsense.org](https://www.pfsense.org)&#x20;

[https://docs.netgate.com/pfsense/en/latest/recipes/rfc1918-egress.html](https://docs.netgate.com/pfsense/en/latest/recipes/rfc1918-egress.html)&#x20;

