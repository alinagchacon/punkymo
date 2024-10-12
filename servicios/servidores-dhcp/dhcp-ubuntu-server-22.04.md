# DHCP-Ubuntu Server 22.04

El servicio de DHCP lo vamos a instalar en una máquina virtual con Ubuntu Server 22.04. En mi caso, voy a utilizar el mismo servidor donde previamente he instalado el servicio de DNS.

### Requisitos previos

* Una MV con Ubuntu Server 22.04&#x20;
* Dos adaptadores de red:
  * NAT o Adaptador Puente
  * Red Interna

### Actualizar e instalar paquetes&#x20;

Actualizamos los repositorios de nuestro sistema:

```
sudo apt update
```

Una vez hecho esto, procedemos  a instalar el paquete isc-dhcp-server:

```
sudo apt install isc-dhcp-server
```

### Configurar DHCP

La configuración de los servicios de una red, se deben planificar con antelación y no lanzarnos a configurar sin antes pensarlo un poco. Siempre podremos **modificarlo a nuestro gusto** pero mejor planificar.&#x20;

Accedemos a la ubicación donde se encuentran los archivos de configuración del servicio, para editar el fichero de configuración de nuestro servidor DHCP. Antes de editar el archivo procedemos a realizar una copia de seguridad:

```
sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.BKP
```

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption><p>Copia del archivo de configuración</p></figcaption></figure>

Tengamos en cuenta algunos aspectos:

* **Rango DHCP**: Se trata de un grupo de direcciones IP para una función determinada dentro de la organización. No confundir con el **ámbito** que es el grupo administrativo de usuarios o equipos de una subred.
* **Reserva de direcciones IP**: Para reservar una o diversas direcciones IP para fines administrativos.
* **Concesión**: Permite establecer el tiempo en el cual un equipo puede usar de forma activa una dirección IP en el dominio o grupo de trabajo.

Editamos el archivo dhcpd.conf, podemos eliminar todo el contenido y reescribir la configuración que te dejo más abajo o bien, habilitar la configuración de la subred interna agregando las opciones necesarias.&#x20;

```
sudo nano /etc/dhcp/dhcpd.conf
```

En la configuración tenemos que aplicar el rango de direcciones IP que queremos que otorgue el servidor DHCP, el dominio que en mi caso es "haven.local" y otros aspectos de la configuración como la IP de broadcast y la IP del router.

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption><p>Fragmento a agregar con los parámetros de nuestra red.</p></figcaption></figure>

En la configuración que hemos aplicado no hemos creada una reserva de direcciones IP para equipos como impresoras u otros servidores. En caso de querer crear una reserva tocaría agregar lo siguiente:

```
host printer {
    hardware ethernet 00:0C:27:C9
    fixed-address 192.168.1.155;
}
```

En la imagen podemos ver otros parámetros de la configuración como por ejemplo:

* el valor de la concesión (Lease-Time)
* varias opciones relacionadas con el **ddns**:
  * El **DDNS** o **DNS dinámico** es un servicio que permite actualizar de manera automática los registros de DNS cuando cambia una dirección IP.  En el servidor DNS se registra la asignación del nombre (del dispositivo) a la dirección IP que se le otorga. No obstante, dado que las direcciones IP cambian con frecuencia, es útil hacer servir el servicio **ddns** que actualiza los registros del servidor DNS cada vez que cambian las direcciones IP. Por tanto, con el **ddns**, la administración de nombres de dominio se vuelve más fácil y eficiente.
  * **ddns-update-style interim**: controla la forma en que se manejan las actualizaciones dinámicas de DNS (DDNS) entre el servidor DHCP y el servidor DNS. Por lo que Esta opción define la fmanera que usará el servidor DHCP para actualizar los registros DNS de modo automático.
  * El estilo **`interim`** automatiza el proceso de actualización de DNS, haciéndolo una opción flexible y útil en entornos donde el servidor DHCP gestiona direcciones IP dinámicas y debe mantener el DNS actualizado sin intervención manual.&#x20;
  * El servidor DHCP y el servidor DNS deben estar bien configurados si queremos que las actualizaciones dinámicas de DNS funcionen correctamente.
  * La opción **`ddns-domainname`** define el **nombre de dominio** que se utilizará cuando el servidor DHCP realice actualizaciones dinámicas de DNS. El nombre de dominio se agrega a los nombres de host de los clientes para formar un nombre de dominio completo (FQDN, Fully Qualified Domain Name) que será registrado en el servidor DNS.
  * Cuando el servidor DHCP asigna una dirección IP a un cliente y está configurado para realizar actualizaciones dinámicas de DNS, el **nombre de host** del cliente se combina con el valor de **`ddns-domainname`** para crear un FQDN que será registrado en el servidor DNS. Este registro puede ser tanto un registro A (nombre a dirección IP) como un registro PTR (dirección IP a nombre de host, para la resolución inversa).
  * Por ejemplo, si el cliente tiene el nombre de host `cliente` y el valor de `ddns-domainname` es `haven.local`, el servidor DHCP registrará el FQDN `cliente.haven.local` en el servidor DNS.

Aquí dejo unos extras de información:

<details>

<summary>¿Cuál es el funcionamiento de interim?</summary>

En el estilo **`interim`**, el servidor DHCP actualiza los registros DNS **directamente y de manera automática** cuando un cliente obtiene o libera una dirección IP. Esto es:

1. **Actualización del registro A (IPv4)**: El servidor DHCP actualizará el registro **A** en el servidor DNS, que mapea un nombre de dominio a una dirección IPv4.
2. **Actualización del registro PTR**: También se actualizará el registro **PTR**, que realiza la resolución inversa (asocia una dirección IP con un nombre de dominio).
3. **Renovación y eliminación**: Cuando un cliente libera una dirección IP o la abandona (por ejemplo, si la conexión DHCP expira o se desconecta), el servidor DHCP eliminará o actualizará los registros DNS correspondientes.

</details>

<details>

<summary>¿Cómo funciona el DDNS? <em>(Tomado de:</em> <a href="https://aws.amazon.com/es/what-is/dynamic-dns/"><em>AWS</em></a><em>)</em></summary>

Las organizaciones suelen suscribirse a un servicio de DNS dinámico (DDNS) proporcionado por un proveedor de DDNS. El proveedor también mantiene los servidores DNS que gestionan los registros de DNS del nombre de dominio asociado. En sentido general es como sigue:

1. Se registra un nombre de dominio con el proveedor de servicios de DNS dinámico y se configuran los ajustes de DNS.
2. Se proporciona al proveedor la dirección IP inicial del nombre de dominio.
3. Se instala un cliente de DNS dinámico en la instancia del dispositivo o servidor con la dirección IP cambiante

El cliente DDNS supervisa de forma continua la dirección IP y detecta cualquier cambio. Envía una notificación de actualización del registro de DNS al proveedor de DNS dinámico, que le informa de la nueva dirección IP. El proveedor de DNS dinámico modifica los registros para que apunten a la nueva dirección IP.

El cliente de DNS dinámico continúa supervisando la dirección IP para detectar cambios adicionales. Cada vez que se produce un nuevo cambio, el proceso se repite.

</details>

### Testeando desde un equipo cliente

Lo siguiente que podemos hacer es comprobar que desde un equipo cliente con Windows o Linux  funciona correctamente. Para ello debemos hacer un release / renew desde la ventana del <mark style="color:blue;">`CMD`</mark>:

```
ipconfig /release
```

y luego&#x20;

```
ipconfif /renew
```

Para liberar y solicitar una nueva configuración de los parámetros de conexión a la red. Esto lo podemos ver en Wireshark, donde se observa claramente el proceso de negociación que se establece entre el cliente y el servidor de DHCP.&#x20;

<figure><img src="../../.gitbook/assets/image (195).png" alt=""><figcaption><p>Captura del tráfico del protocolo DHCP al hacer un release / renew </p></figcaption></figure>

Nota: Tener en cuenta que esta información del Wireshark no se corresponde exactamente con los datos del equipo cliente.

Si todo ha ido bien deberíamos tener un resultado como el que sigue:

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption><p>Parámetros de red en un equipo cliente. Claramente toma la primera IP del rango: 192.168.6.25</p></figcaption></figure>



Podemos ver en el servidor si el cliente tiene una dirección DHCP. Esto lo podemos ver en en el archivo de "<mark style="color:blue;">arrendamientos</mark>":

```
more /var/lib/dhcpd/dhcpd.leases
```

<figure><img src="../../.gitbook/assets/image (21) (1).png" alt=""><figcaption><p>Archivo /var/lib/dhcpd/dhcpd.leases</p></figcaption></figure>

### Algunos errores comunes con DHCP

* Mientras el cliente no encuentra el servidor de  DHCP el sistema operativo del cliente proporciona **automáticamente** una IP que comienza por 169.254.xx hasta que se pone en contacto con el servidor DHCP. Cuando esto ocurre, se restablecen los valores que otorga el servidor. En resumen, si vemos una IP 169.254.xx (apipa) significa que **hay un fallo** en la comunicación con el servidor DHCP.
* Si estamos trabajando en Cisco, puede ser algún detalle mal configurado en el servidor.
* En la vida "real" existen varias causas como por ejemplo: un problema con la conexión física, el cable Ethernet, la tarjeta de red.&#x20;
* Por tanto, las causas pueden ser:
  * Los cables no funcionan.
  * El switch o router al que está conectado el servidor cuenta con su propio servicio DHCP.
  * Configuración incorrecta en nuestro servidor DHCP

