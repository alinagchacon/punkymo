# DHCP-Ubuntu Server 22.04

El servicio de DHCP lo vamos a instalar en una máquina virtual con Ubuntu Server 22.04. En mi caso, voy a utilizar el mismo servidor donde previamente he instalado el servicio de DNS.

### Requisitos previos

* Una MV con Ubuntu Server 22.04&#x20;
* Dos adaptadores de red:
  * NAT o Adaptador Puente
  * Red Interna

### Actualizar e instalar paquetes&#x20;

Actualizamos los repositorios de nuestro sistema:

<mark style="color:blue;">`sudo apt update`</mark>

Una vez hecho esto, procedemos  a instalar el paquete isc-dhcp-server:

<mark style="color:blue;">`sudo apt install isc-dhcp-server`</mark>

### Configurar DHCP

La configuración de los servicios de una red, se deben planificar con antelación y no lanzarnos a configurar sin antes pensarlo un poco. Siempre podremos **modificarlo a nuestro gusto** pero mejor planificar.&#x20;

Accedemos a la ubicación donde se encuentran los archivos de configuración del servicio, para editar el fichero de configuración de nuestro servidor DHCP. Antes de editar el archivo procedemos a realizar una copia de seguridad:

<mark style="color:blue;">`sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.BKP`</mark>&#x20;

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption><p>Copia del archivo de configuración</p></figcaption></figure>

Para editar el archivo utilizo <mark style="color:blue;">`nano`</mark>

<mark style="color:blue;">`sudo nano /etc/dhcp/dhcpd.conf`</mark>&#x20;

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption><p>Fragmento a agregar con los parámetros de nuestra red.</p></figcaption></figure>

En la imagen previa podemos ver los parámetros de la configuración como por ejemplo:

* el valor de la concesión (Lease-Time),&#x20;
* el rango y ámbito,
* el dominio&#x20;

En este caso no tengo creada una reserva de direcciones IP para equipos como impresoras u otros servidores. En ese caso tocaría agregar lo siguiente:

<mark style="color:blue;">`host printer {`</mark>\
&#x20;            <mark style="color:blue;">`hardware ethernet 00:0C:27:C9`</mark>\
&#x20;            <mark style="color:blue;">`fixed-address 192.168.1.155;`</mark>\ <mark style="color:blue;">`}`</mark>&#x20;

Lo siguiente que podemos hacer es comprobar que desde un equipo cliente con Windows  funciona correctamente. Para ello debemos hacer un release / renew desde la ventana del <mark style="color:blue;">`CMD`</mark>:

<mark style="color:blue;">`ipconfig /release`</mark>&#x20;

y luego&#x20;

<mark style="color:blue;">`ipconfig /renew`</mark>&#x20;

Para liberar y solicitar una nueva configuración de los parámetros de conexión a la red. Esto lo podemos ver en Wireshark, donde se observa claramente el proceso de negociación que se establece entre el cliente y el servidor de DHCP.&#x20;

<figure><img src="../../.gitbook/assets/image (195).png" alt=""><figcaption><p>Captura del tráfico del protocolo DHCP al hacer un release / renew </p></figcaption></figure>

Nota: Tener en cuenta que esta información del Wireshark no se corresponde exactamente con los datos del equipo cliente.

Si todo ha ido bien deberíamos tener un resultado como el que sigue:

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption><p>Parámetros de red en un equipo cliente. Claramente toma la primera IP del rango: 192.168.6.25</p></figcaption></figure>



Podemos ver en el servidor si el cliente tiene una dirección DHCP. Esto lo podemos ver en en el archivo de "<mark style="color:blue;">arrendamientos</mark>":

<mark style="color:blue;">`more /var/lib/dhcpd/dhcpd.leases`</mark>

<figure><img src="../../.gitbook/assets/image (21) (1).png" alt=""><figcaption><p>Archivo /var/lib/dhcpd/dhcpd.leases</p></figcaption></figure>

### Algunos errores comunes con DHCP

* Mientras el cliente no encuentra el servidor de  DHCP el sistema operativo del cliente proporciona **automáticamente** una IP que comienza por 169.254.xx hasta que se pone en contacto con el servidor DHCP. Cuando esto ocurre, se restablecen los valores que otorga el servidor. En resumen, si vemos una IP 169.254.xx (apipa) significa que **hay un fallo** en la comunicación con el servidor DHCP.
* Si estamos trabajando en Cisco, puede ser algún detalle mal configurado en el servidor.
* En la vida "real" existen varias causas como por ejemplo: un problema con la conexión física, el cable Ethernet, la tarjeta de red.&#x20;
* Por tanto, las causas pueden ser:
  * Los cables no funcionan.
  * El switch o router al que está conectado el servidor cuenta con su propio servicio DHCP.
  * Configuración incorrecta en nuestro servidor DHCP

