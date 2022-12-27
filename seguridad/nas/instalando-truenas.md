---
description: Apuntes
---

# Instalando Truenas

<mark style="color:blue;">`Truenas`</mark> es un sistema operativo (SO) que está basado en la licencia Berkeley Software Distribution (BSD) y proporciona servicios de almacenamiento en red. Es un SO gratuito, open-source que permite convertir un PC en un soporte de almacenamiento accesible desde red, por ejemplo para almacenamientos masivos de información, copias de seguridad de datos, música, etc.

Truenas:&#x20;

* Se trata de un sistema operativo orientado específicamente para funcionar como un servidor NAS profesional de alto rendimiento.&#x20;
* Se puede instalar en cualquier plataforma x64 gracias a que el sistema operativo base es FreeBSD en su versión 12.&#x20;
* Incorpora alta compatibilidad con gran cantidad de hardware
* Facilita la utilización y la configuración de todos los servicios que debe tener un servidor NAS, como es: Samba, FTP,  almacenamiento con RAID, el acceso remoto vía OpenVPN, etc.



### Instalación en VirtualBox

Vamos a crear una máquina virtual (VM) en <mark style="color:blue;">`Virtualbox`</mark>. Para ello, el detalle a tener en cuenta es el SO a seleccionar para arrancar la VM.

Requerimientos técnicos:

* Sistema operativo: BSD, de 64-bits
* RAM: Requiere 8GB mínimo (se puede crear con menos pero salen mensajes)
* Red: En adaptador puente (para acceder a través de la red a la VM)
* Discos: 3 discos duros como mínimo
* ISO: TrueNAS-13.0-U2.iso

Nos descargamos la versión TRUENAS CORE.

<figure><img src="../../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

Creamos la VM con los datos requeridos, esto es: 4GB de RAM, 3 discos duros para crear un RAID1 y en modo puente para tener acceso a la VM desde otro equipo de la red.

<figure><img src="../../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

Durante el proceso de instalación seleccionamos un disco para realizar la instalación del SO, dejando los otros dos discos libres por el momento. Seleccionamos además el modo de arranque con BIOS porque funciona con prácticamente todas las placas base. Una vez hecho esto, nos pedirá reiniciar el sistema asegurándonos extraer la ISO del medio óptico.

Reiniciamos la VM y debemos visualizar un menú como el siguiente:

<figure><img src="../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>



A través del navegador accedemos a la VM para configurar todos los servicios.

<figure><img src="../../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

Una vez dentro accedemos al dashboard que trae estadísticas de uso de la CPU, del sistema, memoria, etc.

TRUENAS te permite instalar plugins, servicios como SSH, FTP, compartir recursos en redes Windows, crear usuarios y grupos, configurar reglas ACL de los recursos compartidos, configurar VPN entre un largo etcétera.

Algunas de las secciones a considerar para una mínima configuración del sistema serían las que se detallan a continuación.

### &#x20;Cuentas de usuarios

En la sección de <mark style="color:blue;">`Accounts`</mark> podemos crear usuarios y grupos de usuarios. Esto nos permitiría administrarlos cómodamente y aplicar diferentes permisos de acceso a cada uno. La buena gestión de los usuarios y permisos es una de las partes más importantes en la gestión de un servidor NAS, que proporciona los mínimos permisos posibles a los usuarios con el objetivo de evitar posibles problemas de seguridad.

### Sistema

En esta sección podemos configurar la interfaz gráfica de usuario con diferentes perfiles. También podemos cambiar el puerto de administración, los protocolos TLS a utilizar, así como el idioma de la interfaz gráfica de usuario, entre otros.&#x20;

### Tareas

Aquí es donde podemos programar el servidor para programar diferentes tareas, configurando los trabajos en el Cron,  scripts a ejecutar durante el inicio y apagado del sistema, copias de seguridad a través de la herramienta Rsync que permite sincronizar carpetas y archivos, testeos de SMART para comprobar el estado de los discos, etc.

### Directory Services

Este es un punto a considerar dado que podemos configurar el <mark style="color:blue;">`Active Directory AD`</mark> si tenemos una red Windows, así como el <mark style="color:blue;">`LDAP`</mark>, <mark style="color:blue;">`NIS`</mark> y <mark style="color:blue;">`Kerberos`</mark>. Tengamos en cuenta que todos estos protocolos están orientados a la autenticación de los usuarios en el NAS,  con lo que podemos realizar una autenticación basada en AD, LDAP o Kerberos para reutilizar la infraestructura de autenticación de la empresa, y no tener que crear todos los usuarios directamente en el sistema operativo del TRUENAS.

Si hablamos de entornos empresariales tendríamos que considerar  la existencia de un servidor para la autenticación de los usuarios y grupos a los que pertenecen y, de esta forma, enlazar TRUENAS con el <mark style="color:blue;">`AD`</mark> de un Windows Server y obtener las credenciales de acceso de estos servidores, y no tener que exportar todos los usuarios y configuración de los mismos pudiendo autenticarlos en el propio <mark style="color:blue;">`AD`</mark>.

### Sharing - Compartiendo

Aquí es donde podremos configurar diferentes protocolos que nos permitan compartir ficheros y carpetas por la red. Para ello tenemos <mark style="color:blue;">`AFP`</mark>, <mark style="color:blue;">`iSCSI`</mark>, <mark style="color:blue;">`NFS`</mark>, <mark style="color:blue;">`WebDAV`</mark> y <mark style="color:blue;">`SAMBA`</mark>, que es ideal en entornos Windows.&#x20;

### Servicios

En esta sección es donde  podemos activar o desactivar  servicios, y configurar su inicio junto con el servidor NAS. Encontraremos aquí todos  los servicios como: compartir ficheros, OpenVPN tanto en modo cliente como en modo servidor, el SMART, Rsync y más.



### Configurando algunos servicios

#### SMB, RSYNC, SSH

Podemos habilitar algún que otro servicio como: samba o smb, rsync que necesita de ssh para probar lo mínimo del servidor.

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption><p>Algunos de los servicios habilitados</p></figcaption></figure>

Para probar el uso de SSH podemos conectarnos desde el CMD de Windows. Recuerda que el NAS está en modo Adaptador Puente con lo cual tenemos acceso al mismo a través de la red.

Por tema de seguridad no es conveniente brindar acceso al root a través de SSH.

<figure><img src="../../.gitbook/assets/image (5) (2) (2).png" alt=""><figcaption></figcaption></figure>

#### Cuentas de usuarios

Para crear un usuario o grupo de usuarios tenemos que ir a:&#x20;

<mark style="color:blue;">`Accounts / Users / Add`</mark>

Se nos abre un formulario que nos pide datos como el nombre de usuario, contraseña, correo, grupo al que pertenece, permisos, etc.

#### Compartir recursos&#x20;

Para poder acceder a los contenidos de cada usuario a través de la red, se hace imprescindible habilitar el servicio <mark style="color:blue;">`SMB`</mark> <mark style="color:blue;"></mark><mark style="color:blue;"></mark> en el servidor:

<div>

<figure><img src="../../.gitbook/assets/image (1) (5).png" alt=""><figcaption><p>Crear usuario</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/image (9) (3).png" alt=""><figcaption><p>Los permisos </p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/image (6) (3) (2).png" alt=""><figcaption><p>ACL</p></figcaption></figure>

</div>

Tener en cuenta que al configurar las reglas ACL, Access Control List, los permisos pueden ser restrictivos, si no queremos que el usuario Pepe vea o acceda al contenido de Lola. Además, debemos considerar que el usuario u owner@ debe tener acceso a todos sus propios recursos, el grupo o group@ al que pertenezca el usuario puede tener o no restricciones en cuanto a permisos y podemos definir también si el resto de usuarios puede tener acceso aunque limitados a los recursos de otro usuario específico.

Si queremos probar el acceso que tiene un usuario específico podemos ir al explorador de <mark style="color:blue;">`Windows - Red - \\TRUENAS`</mark> y debemos ver algo como:

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>Usuarios </p></figcaption></figure>



Hemos visto algunos aspectos de la configuración del <mark style="color:blue;">**`TrueNAS CORE`**</mark> de un modo muy básico. Falta mucho por configurar para brindar seguridad, fiabilidad y conectividad a los usuarios. Por lo que no puedo dar por acabada la configuración del servidor.

<mark style="color:red;">To be continued ...</mark>



### Links

* [https://www.truenas.com/truenas-enterprise/](https://www.truenas.com/truenas-enterprise/)
* [https://www.redeszone.net/tutoriales/servidores/truenas-core-guia-instalacion-configuracion-nas/ ](https://www.redeszone.net/tutoriales/servidores/truenas-core-guia-instalacion-configuracion-nas/)
