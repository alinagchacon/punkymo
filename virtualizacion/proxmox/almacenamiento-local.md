---
description: En VM
---

# Almacenamiento local

Todo el "problema" que estoy teniendo desde hace un tiempo con el despliegue de VM dentro de Proxmox en VirtualBox es porque lo intento configurar y utilizar a modo de prueba con los mínimos recursos indispensables. Digamos que estoy pensando en cómo lo pudieran probar mis alumnos si no cuentan con ciertos recursos.

En este apartado estoy partiendo de la base que tengo Proxmox instalado como VM en VirtualBox con las siguientes condiciones.

| Componentes | PROXMOX         | MV Ubuntu Server 22.04 |
| ----------- | --------------- | ---------------------- |
| RAM         | 8GB             | 2GB                    |
| CPU         | 4               | 1                      |
| HDD         | 60GB            | 12GB                   |
| RED         | NAT - 10.0.2.15 | VMBR0 - 10.0.2.16      |

Para poder acceder al sistema desde el navegador no queda otra que hacer un reenvío de puertos:

<figure><img src="../../.gitbook/assets/image (366).png" alt=""><figcaption><p>Reenvío de puertos en red NAT</p></figcaption></figure>

En este Proxmox instalado ya tengo configurado un contenedor **LXC de Debian** y la **VM de Ubuntu Server**. Sin embargo, al querer instalar otra VM de Ubuntu con las mismas condiciones, se me "crashea" Proxmox y se cierra de manera inesperada. ¿Por qué? Pienso que es por la siguiente razón:

Si nos vamos a: Datacenter - Storage vemos algo así:

<figure><img src="../../.gitbook/assets/image (367).png" alt=""><figcaption><p>Storage en Proxmox</p></figcaption></figure>

**local**: es de tipo directorio y nos permite almacenar, archivos de backups, imágenes ISO y plantillas de contenedores.

**local-lvm**: es un volumen lógico

Si queremos crear una VM y almacenarla en **local** no lo podemos hacer porque no nos dejaría almacenar imágenes de disco. Cosa que si permitiría el volumen lógico **local-lvm**.&#x20;

Analizando los espacios utilizados tanto para local como local-lvm veo lo siguiente:

* local: utilizado 4.91GB de 26.69GB
* local-lvm: utilizado 5.17GB de 20.30GB

No sé si es un problema de espacio para otra VM o es un problema de la CPU, por ejemplo, así que comienzo por el tema del espacio.

### ¿Qué voy a hacer?&#x20;

Pues probar que eliminando el volumen lógico **local-lvm** y asignando ese espacio a **local** podremos tener espacio como para configurar más de dos VM en Proxmox aunque no sea lo más aconsejable. Piensa que ahora tendríamos mezclados el sistema operativo con las VM que tengamos instaladas.

Comencemos por eliminar el volumen lógico desde la línea de comandos. Puedes investigarlos:

<figure><img src="../../.gitbook/assets/image (370).png" alt=""><figcaption><p>Comandos que pueden ser útiles </p></figcaption></figure>

Primero podemos listar los volúmenes lógicos que tenemos con el comando lvs, esto es:

```
lvs
```

<figure><img src="../../.gitbook/assets/image (368).png" alt=""><figcaption><p>Listado de volúmenes lógicos</p></figcaption></figure>

Ahora podemos eliminar el volumen que queremos:

```
lvremove /dev/pve/data
```

<figure><img src="../../.gitbook/assets/image (369).png" alt=""><figcaption><p>Eliminando el volumen lógico</p></figcaption></figure>

El siguiente paso es hacer un resize de modo que añadimos el 100% del espacio libre a pve/root:

```
lvresize -l 100%FREE /dev/pve/root
```

<figure><img src="../../.gitbook/assets/image (371).png" alt=""><figcaption></figcaption></figure>

FInalmente, hacemos un resize del file system, así que:

```
resize2fs /dev/mapper/pve-root
```

<figure><img src="../../.gitbook/assets/image (372).png" alt="" width="553"><figcaption></figcaption></figure>

Una vez hecho esto, nos volvemos al sistema y vemos si se da cuenta de los cambios o lo reiniciamos para que los asuma:

<figure><img src="../../.gitbook/assets/image (373).png" alt="" width="370"><figcaption><p>Status unknowm </p></figcaption></figure>

Asi que reiniciamos el sistema con un **reboot**.

Una vez reiniciado el sistema vamos de nuevo a Datacenter - Storage y podemos eliminar el volumen lógico: local-lvm porque realmente ya no existe.

Una vez "removed" el volumen lógico podemos editar el local y permitir que almacene de todo tipo de archivos:

<figure><img src="../../.gitbook/assets/image (374).png" alt="" width="375"><figcaption><p>Editando datacenter-storage-local</p></figcaption></figure>

De este modo hemos maximizado el almacenamiento en Proxmox.

<figure><img src="../../.gitbook/assets/image (375).png" alt="" width="563"><figcaption><p>En pve - summary</p></figcaption></figure>

### Desplegando VM

He vuelto a desplegar una VM con las condiciones antes descritas y como se muestra en la imagen a continuación. Como se puede observar, le asigné 2 cores y aún así me salió el mismo error crítico.

<figure><img src="../../.gitbook/assets/image (376).png" alt=""><figcaption><p>Características de hardware de la VM de Ubuntu Server 22.04</p></figcaption></figure>

La solución ha sido rebajar los cores de la CPU y dejarlo solo en uno.

&#x20;

<figure><img src="../../.gitbook/assets/image (377).png" alt=""><figcaption><p>Características de la VM de Ubuntu Server 22.04</p></figcaption></figure>

### Links

* Interesante este curso: [https://www.youtube.com/watch?v=IB\_M61PQp1k\&t=1s](https://www.youtube.com/watch?v=IB\_M61PQp1k\&t=1s)&#x20;
