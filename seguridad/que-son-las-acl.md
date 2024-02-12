# ¿Qué son las ACL?

Las ACL especifican los usuarios o procesos  del sistema que tienen determinado tipo de acceso y las operaciones que pueden realizar en los objetos dados: ficheros, aplicaciones, directorios. Cada entrada especifica un tema y una operación.

#### ¿Cómo funcionan?

El sistema debe de tener habilitado las ACL, para lo cual  debemos especificar si se montan de manera <mark style="color:blue;">provisional</mark> o <mark style="color:blue;">permanente</mark> en el fichero <mark style="color:blue;">`/etc/fstab`</mark>

#### El sistema operativo

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption><p>El sistema donde lo estoy probando</p></figcaption></figure>

Para comprobar si las ACL están habilitadas en las particiones montadas, podemos ejecutar el siguiente comando:

<mark style="color:blue;">tune2fs -l /dev/sda2</mark>&#x20;

pero podemos acotar la información que nos muestra utilizando el comando grep en modo "pipe", esto es:

<mark style="color:blue;">`tune2fs -l /dev/sda2 | grep "Default mount options:"`</mark>

<figure><img src="../.gitbook/assets/image (159).png" alt=""><figcaption><p>tune2fs</p></figcaption></figure>

#### Instalando las ACL en el sistema

<mark style="color:blue;">`sudo apt-get update`</mark>

<mark style="color:blue;">`sudo apt-get install acl`</mark>

Si tienes dudas de qué hacer y cómo en cuanto a la instalación de las ACL en el sistema, en este [<mark style="color:blue;">`enlace`</mark> ](https://installati.one/ubuntu/21.10/acl/)puedes verlo. Además, recuerda que puedes encontrar toda la información utilizando el comando <mark style="color:blue;">man</mark>. Por ejemplo:

<mark style="color:blue;">`man acl`</mark>

<mark style="color:blue;">`man getfacl`</mark>

#### Implementando las ACL

Para implementar las ACL en el sistema, disponemos de dos herramientas:

1. <mark style="color:blue;">GETFACL</mark> - Muestra información de los permisos de ficheros y carpetas.&#x20;
2. <mark style="color:blue;">SETFACL</mark> - Establece las ACL de ficheros y directorios&#x20;
3. <mark style="color:blue;">CHACL</mark>  - Modifica las ACL de ficheros y directorios

#### Listar permisos de ficheros y carpetas&#x20;

Con el comando <mark style="color:blue;">getfacl</mark> podemos listar los permisos de los ficheros y carpetas. Para ello, basta con ejecutar el comando:

<mark style="color:blue;">`getfacl directorio`</mark>

En mi caso, creé un directorio y agregué un fichero dentro llamado readme. Esto es:

mkdir testingACL

touch testingACL/readme

nano testingACL/readme&#x20;

Y escribí un contenido cualquiera a modo de prueba.&#x20;

Un modo mucho más rápido (al que debería acostumbrarme) es:

<mark style="color:blue;">`echo "Este es mi fichero de prueba" > testingACL/fichero`</mark>

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption><p>crear fichero con el comando echo</p></figcaption></figure>

Por tanto, para listar el contenido de un fichero como tal hacemos:

<mark style="color:blue;">`getfacl testingACL/readme`</mark>&#x20;

<figure><img src="../.gitbook/assets/image (184).png" alt=""><figcaption><p>getfacl</p></figcaption></figure>

Otro ejemplo, donde estoy listando los permisos de /home y /etc:

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption><p>getfacl</p></figcaption></figure>

#### Agregar permisos a un usuario

Podemos ver las opciones del comando setfacl haciendo:

setfacl --help

<figure><img src="../.gitbook/assets/image (152).png" alt=""><figcaption><p>Opciones de setfacl</p></figcaption></figure>

Supongamos que queremos permitir que el usuario <mark style="color:blue;">`pepe`</mark> (si no tienes otro usuario en el sistema tienes que crearlo con el comando <mark style="color:blue;">`adduser`</mark>) tenga acceso  de lectura y escritura al fichero <mark style="color:blue;">`readme`</mark>, entonces:

<mark style="color:blue;">`sudo setfacl -m u:pepe:rw  /home/punky/testingACL/readme`</mark>

<figure><img src="../.gitbook/assets/image (191).png" alt=""><figcaption><p>setfacl</p></figcaption></figure>

¿Qué sucede? ¿Puedes visualizar el contenido de readme?

Prueba a otorgar los permisos de manera recursiva al directorio de prueba.



#### Links

1. [https://www.linuxfordevices.com/tutorials/linux/tune2fs-command](https://www.linuxfordevices.com/tutorials/linux/tune2fs-command)&#x20;
2. [https://manpages.ubuntu.com/manpages/trusty/man5/acl.5.html](https://manpages.ubuntu.com/manpages/trusty/man5/acl.5.html)
3. [https://manpages.ubuntu.com/manpages/trusty/man1/getfacl.1.html](https://manpages.ubuntu.com/manpages/trusty/man1/getfacl.1.html)
4. [https://installati.one/ubuntu/21.10/acl/](https://installati.one/ubuntu/21.10/acl/)
5. [https://juncotic.com/acl-access-control-lists-y-los-permisos-en-gnu-linux/](https://juncotic.com/acl-access-control-lists-y-los-permisos-en-gnu-linux/)&#x20;
6. [https://www.ochobitshacenunbyte.com/2019/02/07/listas-de-control-de-acceso-acl-en-linux/](https://www.ochobitshacenunbyte.com/2019/02/07/listas-de-control-de-acceso-acl-en-linux/)
