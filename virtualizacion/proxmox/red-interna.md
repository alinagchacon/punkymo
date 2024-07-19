---
description: Proxmox
---

# Red Interna

He estado testeando Proxmox en diferentes condiciones pero siempre las mínimas. Me refiero a tener:&#x20;

* espacio en disco no más de 100GB.&#x20;
* una única interfaz de red
* VirtualBox como hipervisor

Se trata de utilizar una única interfaz de red para tener acceso a Internet y una red interna para varias VM en Proxmox. Para ello, seguí la guía de Proxmox en: [https://pve.proxmox.com/wiki/Network\_Configuration](https://pve.proxmox.com/wiki/Network\_Configuration)&#x20;

<figure><img src="../../.gitbook/assets/image (241).png" alt=""><figcaption><p>Red interna en Proxmox</p></figcaption></figure>

En esta guía que os recomiendo, una de las opciones es el enmascaramiento `NAT` con iptables. Este enmascaramiento nos permite el acceso a la red utilizando la dirección IP del host para el tráfico saliente, teniendo una dirección IP privada, como es el caso.

Utilizamos `IPTABLES` para reescribir cada paquete saliente de modo que parezca que se origina en el host. Naturalmente, las respuestas se reescriben para enrutarse al remitente original.

De hecho, la guía ha sido de la misma wiki de Proxmox:

<figure><img src="../../.gitbook/assets/image (242).png" alt=""><figcaption><p><a href="https://pve.proxmox.com/pve-docs/images/default-network-setup-routed.svg">https://pve.proxmox.com/pve-docs/images/default-network-setup-routed.svg</a></p></figcaption></figure>

¡Vamos a comenzar!

### Configurando una red interna en Proxmox

Lo primero es tener bien claro cuál es la configuración inicial con la que estoy trabajando. Me resulta muy importante tener claro el esquema de la configuración.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Diagrama de la red</p></figcaption></figure>

El test lo vamos a hacer con dos VM de Ubuntu donde una hará de router de la otra.&#x20;

| Proxmox                     | VM Ubuntu Router        | VM Ubuntu Cliente       |
| --------------------------- | ----------------------- | ----------------------- |
| IP (estática): 10.0.2.15/24 | IP (dhcp): 10.0.2.16/24 | IP (dhcp): 10.0.2.17/24 |
| IP gateway: 10.0.2.2        | IP gateway: 10.0.0.2    | IP gateway: 10.0.0.2    |
| Red: NAT                    | Red: vmbr0              | Red: vmbr0              |



La VM de Proxmox tiene una IP estática configurada durante el proceso de instalación. Dicha VM está conectada en red: NAT y como he dicho anteriormente, es el único modo en que he podido instalar VM o contenedores de Linux LXC y que todos tengan una IP y salida a Internet.

### Paso 1: Nueva interfaz de red en Proxmox

Por defecto, tenemos el linux bridge virtual **vmbr0** que es el que se conecta a nuestra interfaz de red  física y que toma del router físico una IP por DHCP.  Como queremos crear una red interna, tenemos que añadir una nuevo linux bridge al que conectaremos el equipo cliente.  Para ello vamos al nodo pve - network - add (Linux Bridge).

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Añadir un nuevo linux bridge: vmbr1</p></figcaption></figure>

Lo único que tendremos que configurar es la IP de la red, no es necesario asignar una IP de gateway. De hecho, el gateway solo debe estar una vez aunque tengamos configuradas varias interfaces de red.

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="563"><figcaption><p>La IP del Proxmox en el nuevo linux bridge es: 10.10.10.253/24</p></figcaption></figure>

Nos debe quedar algo como lo siguiente:

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Los dos linux bridge: vmbr0 y vmbr1</p></figcaption></figure>



### Paso 2: vmbr0 y vmbr1

La VM router estará conectada a los dos dos linux bridges: vmbr0 y vmbr1 . Recordemos que será esta VM la que le da Internet al equipo cliente.&#x20;

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>VM router conectada a los dos linnux bridge</p></figcaption></figure>

En el caso de la VM cliente debemos conectarla (bastaría con editar el  linux bridge vmbr0)  para que esté conectada al vmbr1 que es el switch de nuestra red interna.

### Paso 3: Configurar las VM que harán de Router y de Cliente

Pasemos a las máquinas. Realmente las dos VM: Router y Cliente han sido clonadas de una VM de Ubuntu que tengo como plantilla.&#x20;

<figure><img src="../../.gitbook/assets/image (1).png" alt="" width="401"><figcaption><p>Las dos VM Router y Cliente como clones "dependientes" de la plantilla "ubuse1"</p></figcaption></figure>



Dado que las VM router y cliente son clones de la VM de Ubuntu Server  (ubuse1) que tengo como plantilla, podríamos cambiarle el nombre de cada una. Para ello, iniciamos ambas VM y editamos el siguiente archivo en cada una:

```
nano /etc/hostname
```

Escribimos el nombre que le corresponda: router o cliente y reiniciamos para que se implementen los cambios correspondientes. Una vez restablecidas las VM debemos ver algo como lo siguiente, en la **VM router**, donde todavía no hemos configurado la IP de la interfaz de red que se conecta al vmbr1.

<figure><img src="../../.gitbook/assets/image (6).png" alt="" width="563"><figcaption><p>Configuración de red de la VM router. La interfaz ens18 es la que se conecta al vmbr0</p></figcaption></figure>

A la **VM cliente** si debemos configurarle la IP en modo estático porque no tenemos ningún servidor de DHCP que le brinde la IP. Tendría que hacerlo la VM router pero tampoco se lo hemos configurado. Por tanto, tenemos que asignarle una IP estática en la red interna: 10.10.10.2/24 en la interfaz de red que se habilita: **ens18** y el **gateway** que le asignamos será la IP del router: **10.10.10.1.**

<figure><img src="../../.gitbook/assets/image (7).png" alt="" width="563"><figcaption><p>Configuración de red de la VM cliente en la red interna con la interfaz ens18</p></figcaption></figure>

### Paso 4: Configurando la red interna

Todo el trabajo de redirigir el tráfico de datos lo tiene que hacer la VM que hace de router así que volvamos a ella.&#x20;

**VM router** - Lo primero es configurar la IP estática para la nueva interfaz de red que le hemos habilitado: en este caso es la **ens19** y le asignamos la IP 10.10.10.1/24.

<figure><img src="../../.gitbook/assets/image (9).png" alt="" width="485"><figcaption><p>Configuración de la red para la VM router en la interfaz de red ens19 conectada al vmbr1</p></figcaption></figure>

Todavía con esto no podemos hacer que el cliente tenga conexión a Internet.



### Paso 5: IPTables :smile:

Lo primero será habilitar el IP forwarding y para ello nos vamos a editar el archivo /etc/systcl.conf y quitar el # en la línea: **net.ipv4.ip\_forward=1**

```
nano /etc/systcl.conf 
```

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption><p>/etc/systcl.conf</p></figcaption></figure>

**iptables** - Esta si es la herramienta que nos hará el trabajo final. Para ello, tendremos que instalarla primero:

```
sudo apt install iptables
```

Cuando termine podemos comprobar que no tenemos ninguna regla habilitada, por supuesto:

```
iptables -L
iptables -t nat -L
```



Ahora configuramos una regla de iptables como se muestra a continuación.&#x20;

```
iptables -t nat -A POSTROUTING -o ens18 -j MASQUERADE
```

Esta regla nos quiere decir que:

<mark style="color:red;">falta</mark>



### Paso 6: Testeando la conexión

Este es el momento en que debemos testear la conexión de la VM cliente a Internet a través de la VM router. Si hacemos un ping a google:

```
ping google.com
ping amazon.es
```

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Ping desde la Vm cliente a google.com en Internet</p></figcaption></figure>

Como se puede ver ya tenemos salida desde el equipo cliente hacia Internet a través de la VM router.

### Links

* [https://www.youtube.com/watch?v=sGdhakDeQyo](https://www.youtube.com/watch?v=sGdhakDeQyo) (excelente video)
* OpenWebinars: Proxmox VE: Redes
* [https://coda.io/@julia-asensio-pedrero/proxmox/introduccion-a-las-redes-en-proxmox-22](https://coda.io/@julia-asensio-pedrero/proxmox/introduccion-a-las-redes-en-proxmox-22)
* [https://www.speaknetworks.com/enable-intel-vt-amd-v-support-hardware-accelerated-kvm-virtualization-extensions/](https://www.speaknetworks.com/enable-intel-vt-amd-v-support-hardware-accelerated-kvm-virtualization-extensions/)
* [https://www.debian.org/doc/manuals/debian-reference/ch05.es.html](https://www.debian.org/doc/manuals/debian-reference/ch05.es.html)
* [https://millaredos.com/proxmox-configurar-internet-una-sola-interfaz-de-red/](https://millaredos.com/proxmox-configurar-internet-una-sola-interfaz-de-red/)

