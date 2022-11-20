# NAS

Llamamos sistema NAS a los dispositivos de almacenamiento que están conectados a la red. Estos dispositivos o servidores de Backup permiten guardar y recuperar datos de un modo centralizado.  Este tipo de dispositivo son flexibles y expansibles, facilitando el aumento de la capacidad de almacenamiento a medida que lo vayamos necesitando.

Un dispositivo NAS es como tener una nube privada en la oficina o en casa. Es más veloz, menos caro y ofrece todos los beneficios de una nube pública, además de brindarnos todo el control del sistema.

Los sistemas NAS son ideales para las pequeñas y medianas empresas. Para el caso de grandes empresas tendríamos que pasar a una infraestructura de red de tipo SAN.

Entre las funcionalidades más habituales que podemos encontrar en dispositivos NAS tenemos:

* Copias de seguridad&#x20;
* Fácil implementación
* Coste relativamente reducido.
* Centralización del almacenamiento de datos de forma segura

Dado que los datos siempre estarán accesibles, facilita la colaboración entre empleados de una empresa. Sin embargo, también podemos acceder a los datos a distancia mediante una conexión en red, facilitando el teletrabajo.

Un NAS puede considerarse como tu centro de Backup o como la unidad de almacenamiento para todos los archivos importantes, como fotografías, vídeos y música. Recomendable para instalarlo en tu casa.

### ¿Qué dispositivo NAS debemos elegir?

Existen varios tipos de servidores NAS en el mercado bastante famosas:&#x20;

#### **QNAP** (Quality Network Appliance Provider).&#x20;

* Es una marca líder que se caracteriza por la fiabilidad, facilidad de uso y gran capacidad de almacenamiento.&#x20;
* Tiene un  sistema operativo denominado QNAP Turbo Station (QTS) que es considerado uno de los mejores dentro de los servidores NAS.&#x20;
* Es un sistema de nivel básico y medio, compatible con Linux ext4.&#x20;
* Ofrece ZFS, que es un sistema de archivos de los más avanzados que destaca por gran capacidad y seguridad en relación a la integridad de los datos y alto rendimiento en lectura y escritura.
* Permite aplicaciones como los servidores de multimedia Plex.
* Solo admiten sistemas RAID tradicionales que deben ser iguales.&#x20;

**Synology**

* NAS fabricado por Synology Inc.
* Cuenta con su propia interfaz de usuario de escritorio incluida en el sistema operativo propietario.
* El sistema operativo de Synology es el denominado Diskstation Manager (DSM).
* Soporta sistemas RAID tradicionales, además de los sistemas SHR (Synology Hybrid RAID) que es el sistema propietario exclusivo de Synology.
* Por defecto Synology siempre nos ofrece una configuración SHR mientras que si queremos realizar un RAID debemos configurarlo manualmente.
* Si vamos a configurar un disco por primera vez utilizando Synology tendríamos que decidir el tipo de sistema (SHR o RAID) y en función de la configuración elegida estaremos obligados a continuar con la opción seleccionada.
* Dispone de todas las funciones de ext4 y puede utilizar el sistema de archivos BTRFS.
* Soporta la verificación de la integridad de los datos en segundo plano que facilita la aceleración del establecimiento y la reconstrucción del RAID.
*

Este tipo de sistemas de manera general, te permiten:

* Administrar archivos
* Virtualizar
* Gestionar usuarios
* Administrar sistemas
* Realizar copias de seguridad
* Organizar archivos multimedia

Pero no nos quedemos con estas opciones, podemos también elegir otros sistemas operativos para NAS como es el caso:

* Truenas
* Open Media Vault: Una versión del sistema para NAS basada en Debian 11.

#### TRUENAS

Es un sistema operativo diseñado específicamente para funcionar como un servidor NAS profesional de alto rendimiento. Se puede instalar en cualquier plataforma x64 dado que el sistema operativo base es FreeBSD en su versión 12.&#x20;

El sistema incorpora compatibilidad con gran cantidad de hardware: motherboards y tarjetas de red y brinda las funcionalidades típicas de un servidor NAS como:&#x20;

* Almacenamiento con RAID
* Servidores Samba y FTP&#x20;
* NFS&#x20;
* Acceso remoto vía OpenVPN
* Proporciona sistema de archivos ZFS (OpenZFS)
* Permite configurar RAID-Z y facilitar así la protección de la información de un posible problema de hardware en los discos.

#### Open Media Vault

Es un sistema operativo para servidores NAS que permite disponer de todas las funciones para almacenamiento en red. Incluye funcionalidades como:

* soporte para protocolos SSH para conexión remota segura para su administración
* SFTP y FTP para subir o descargar archivos
* RSync para sincronización de archivos
* Tiene un diseño modular que te permite agregar más funciones gracias a sus plugin.
* Teniendo en cuenta que viene de Debian como sistema operativo, nos proporciona seguridad y estabilidad.&#x20;

### Links

* [https://www.qnap.com/es-es](https://www.qnap.com/es-es)&#x20;
* [https://www.synology.com/es-es](https://www.synology.com/es-es)
* [https://www.synology.com/es-es/dsm/overview/virtualization](https://www.synology.com/es-es/dsm/overview/virtualization)
* [https://www.redeszone.net/tutoriales/servidores/sistemas-archivos-ext4-btfs-zfs-elegir/](https://www.redeszone.net/tutoriales/servidores/sistemas-archivos-ext4-btfs-zfs-elegir/) <mark style="color:red;">\*</mark>



