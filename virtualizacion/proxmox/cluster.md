---
description: Creando un clúster en Proxmox
---

# Clúster

A la hora de configurar un clúster necesitamos tener dos servidores de Proxmox.

Una primera VM con Proxmox tiene la configuración siguiente:

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption><p>Servidor 1 de Proxmox</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption><p>Archivos /etc/hosts y /etc/network/interfaces del servidor Kirby</p></figcaption></figure>

Una segunda VM con Proxmox sería la siguiente:

<figure><img src="../../.gitbook/assets/image (223).png" alt=""><figcaption><p>Servidor 2 de Proxmox</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (226).png" alt=""><figcaption><p>Archivos /etc/hosts y /etc/network/interfaces del servidor Punky</p></figcaption></figure>

En ambos casos hay que tener en cuenta que los archivos /etc/hosts deben apuntar a la IP que realmente tienen las VM. Adicionalmente, los nombres de los nodos deben ser diferentes. En caso de que hubiera clonado la VM puedes renombrar el nodo editando los ficheros y reiniciando el sistema:

* /etc/hosts
* /etc/hostname

Por tanto, tenemos dos servidores con IP: 192.168.1.150 y 192.168.1.152

Una vez que tengamos esto vamos al primer servidor de Proxmox que llamé Kirby, y en la sección de: `Datacenter > clúster > crear clúster`  creamos el clúster.&#x20;

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Creando el clúster</p></figcaption></figure>

En este mismo servidor `Kirby`, clicamos en `Join information` para copiar la información que nos muestra:

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Copiar la información que nos permitirá unir el segundo servidor al clúster ya creado</p></figcaption></figure>

Con esta información es que podremos agregar el segundo servidor de Proxmox llamado Punky, al clúster ya creado en el primer servidor.&#x20;

Para ello, nos vamos a lo que sería nuestro segundo nodo y en `Datacenter > Clúster` seleccionamos `Join cluster`:

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption><p>Uniéndonos al clúster creado en el primer servidor de Proxmox</p></figcaption></figure>

Pegamos la información que hemos copiado del primer nodo del clúster, agregamos el password de acceso de root del Proxmox que funciona como primer nodo y le damos al `Join` para establecer el clúster.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Tener en cuenta que estas IP no se corresponden exactamente</p></figcaption></figure>

Ahora, si nos vamos al servidor Kirby que es el primer servidor de Proxmox donde creamos el clúster veremos lo siguiente:

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption><p>Servidor Kirby</p></figcaption></figure>

Y en el segundo servidor veríamos algo semejante:

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption><p>Servidor Punky</p></figcaption></figure>

De este modo, ya pudiéramos trabajar en ambos servidores, migrar VM de servidor a servidor, tener mayor disponibilidad de recursos, configurar ambos servidores bajo el mismo firewall, configurar un backup para ambos servidores, etc.

&#x20;

