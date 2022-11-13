# RAID

Vamos a crear un RAID 1 por software utilizando una máquina virtual en Virtual Box, para añadir discos con facilidad; todos los pasos son válidos para una máquina real, porque el SO no sabe que se está ejecutando en una máquina virtual.

Antes de arrancar la VM, crearemos los discos que queremos conectar. Para ello creamos nuestra VM y le agregamos un disco duro de modo que tengamos dos para poder crear nuestra copia espejo.

En realidad deberíamos poder crear el raid 1 por la línea de comandos, utilizando la herramienta <mark style="color:blue;">`mdadm`</mark> para gestionar el raid. Sin embargo, podemos hacerlo también durante el proceso de instalación del SO.

El proceso es similar a cualquier instalación hasta el momento en que tenemos que establecer la estructura del disco, que es donde se encuentran realmente las diferencias más importantes.

Sin embargo, en este caso elegiremos la opción _Manual_, que nos permite seleccionar o reorganizar las particiones de todos los discos presentes en el sistema.

Debemos ver dos discos:

SCSI1 (0,0,0) (sda) - 8,6 GB ATA VBOX HARDDISK

SCSI2 (0,0,0) (sdb) - 8,6 GB ATA VBOX HARDDISK

Al llegar al punto de tener que particionar los discos seleccionamos lo más sencillo en este caso, que sería "<mark style="color:blue;">`todos los ficheros en una partición`</mark>" que sería lo recomendado para novatos.

Crear las particiones  es un paso indispensable en el proceso de instalación. Ya sabemos que particionar un disco consiste en dividir el espacio disponible donde cada división se denomina <mark style="color:blue;">`partición`</mark>` ``` y esto se debe realizar según los datos que serán almacenados en él y el uso propuesto para el equipo.&#x20;

Este proceso también incluye elegir el sistema de archivo que será utilizado. Todas estas decisiones influirán en el rendimiento, la seguridad de los datos y la administración del servidor.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Por tanto, es necesario definir las particiones del disco y el sistema de archivos que en el caso de Linux sería ext4 y la swap, que es la memoria virtual.&#x20;

El software de particionado proporciona un modo <mark style="color:blue;">`guiado`</mark> que recomienda las particiones que debe crear el usuario — en la mayoría de los casos puede simplemente aceptar las sugerencias del software.

* El sistema de archivos: valor predeterminado es _ext4._
* El punto de montaje: representa el punto del árbol de directorios donde se sitúa la nueva partición. Será el lugar donde se instale el sistema, por lo que dejamos también su valor predeterminado, que es el directorio raíz(/).

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>



Una vez finalizado este proceso debemos obtener el raid montado en las particiones:

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Finalizamos el particionado y continuamos con el proceso de instalación. Una vez reiniciado el sistema, podemos verificar el montaje del raid en el sistema. Para ello podemos verificar el archivo /proc/mdstat que es un archivo especial que nos muestra el estado del controlador md del kernel de Linux.&#x20;

El controlador md o <mark style="color:blue;">`dispositivo múltiple`</mark> es la implementación RAID de software que permite crear cualquier número de dispositivos RAID en función de los dispositivos de disco (físicos o virtuales) disponibles para en el sistema.

cat /proc/mdstat

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

mdadm -D  /dev/md0

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>



Así tendríamos configurado un RAID1. Cierto que interesante sería probarlo directamente con la herramienta mdadm.



### Links

* [https://jesusfernandeztoledo.com/raid-1-en-linux/](https://jesusfernandeztoledo.com/raid-1-en-linux/)&#x20;
* [https://www.nosolohacking.info/configurar-cualquier-raid-en-cualquier-distro-de-linux/](https://www.nosolohacking.info/configurar-cualquier-raid-en-cualquier-distro-de-linux/)
* [https://blogsaverroes.juntadeandalucia.es/profemaria/files/2017/02/ubunturaid.pdf](https://blogsaverroes.juntadeandalucia.es/profemaria/files/2017/02/ubunturaid.pdf)
* [https://debian-handbook.info](https://debian-handbook.info)





