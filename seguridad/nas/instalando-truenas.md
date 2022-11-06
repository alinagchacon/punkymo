---
description: Apuntes
---

# Instalando Truenas

Truenas es un sistema operativo (SO) que está basado en la licencia Berkeley Software Distribution (BSD) y proporciona servicios de almacenamiento en red. Es un SO gratuito, open-source que permite convertir un PC en un soporte de almacenamiento accesible desde red, por ejemplo para almacenamientos masivos de información, copias de seguridad de datos, música, etc.

### Instalación en VirtualBox

Vamos a crear una máquina virtual (VM) en Virtualbox. Para ello, el detalle a tener en cuenta es el SO a seleccionar para arrancar la VM.

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

El menú te permite configurar cuentas de usuarios, grupos, .....

<mark style="color:red;">To be continued ...</mark>

&#x20;





### Links

* [https://www.truenas.com/truenas-enterprise/](https://www.truenas.com/truenas-enterprise/)
