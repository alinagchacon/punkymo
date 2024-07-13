---
description: En VM
---

# Almacenamiento local

Todo el "problema" que estoy teniendo desde hace un tiempo con el despliegue de VM dentro de Proxmox en VirtualBox es porque lo intento configurar y utilizar a modo de prueba con los mínimos recursos indispensables. Digamos que estoy pensando en cómo lo pudieran probar mis alumnos si no cuentan con ciertos recursos.

<mark style="color:orange;">Falta por explicar</mark>

En este apartado estoy partiendo de la base que tengo Proxmox instalado como VM en VirtualBox con las siguientes condiciones.

* RAM: 8GB
* CPU: 4
* HDD: 60GB
* Red: NAT&#x20;

Para poder acceder al sistema desde el navegador no queda otra que hacer un reenvío de puertos:

<figure><img src="../../.gitbook/assets/image (366).png" alt=""><figcaption><p>Reenvío de puertos en red NAT</p></figcaption></figure>

### ¿Qué sucede?

En este Proxmox instalado configuré un contenedor LXC de Debian y  VM de Ubuntu con 15GB de RAM. Sin embargo, al querer instalar otra VM de Ubuntu con las mismas condiciones, se me crasheaba Proxmox y se cerraba de manera inesperada. ¿Por qué? Pienso que es por la siguiente razón:

Si nos vamos a: Datacenter - Storage vemos algo así:

<figure><img src="../../.gitbook/assets/image (367).png" alt=""><figcaption><p>Storage en Proxmox</p></figcaption></figure>

**local**: es de tipo directorio y nos permite almacenar, archivos de backups, imágenes ISO y plantillas de contenedores.

**local-lvm**: es un volumen lógico

Si queremos crear una VM y almacenarla en **local** no lo podemos hacer porque no nos dejaría almacenar imágenes de disco. Cosa que si permitiría el volumen lógico **local-lvm**.&#x20;

### ¿Qué voy a hacer?&#x20;

Pues probar que elimando el volumen lógico local-lvm y asignando ese espacio a local podremos tener espacio como para configurar más de dos VM en Proxmox.



### Links

* Interesante este curso: [https://www.youtube.com/watch?v=IB\_M61PQp1k\&t=1s](https://www.youtube.com/watch?v=IB\_M61PQp1k\&t=1s)&#x20;
