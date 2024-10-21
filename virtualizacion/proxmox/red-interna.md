---
description: Proxmox
---

# Red Interna

He estado testeando Proxmox en diferentes condiciones pero siempre con las mínimas y me refiero a:&#x20;

* espacio en disco no más de 100GB.&#x20;
* una única interfaz de red.
* en VM con VirtualBox como hipervisor.

Se trata de utilizar una única interfaz de red para tener acceso a Internet y una red interna para varias VM en Proxmox. Para ello, estuve  siguiendo la guía de Proxmox en:&#x20;

{% embed url="https://pve.proxmox.com/wiki/Network_Configuration" %}
Wiki de Proxmox
{% endembed %}

En esta guía que os recomiendo, una de las opciones es el enmascaramiento `NAT` con `IPTABLES`. Este enmascaramiento nos permite el acceso a la red utilizando la dirección IP del host para el tráfico saliente, teniendo una dirección IP privada, como es el caso.

Por tanto, la idea es utilizar `IPTABLES` para reescribir cada paquete saliente de modo que parezca que se origina en el host. Naturalmente, las respuestas se reescriben para enrutarse al remitente original.

Según la wiki de Proxmox el diagrama de red a simular es el siguiente:

<figure><img src="../../.gitbook/assets/image (242).png" alt=""><figcaption><p><a href="https://pve.proxmox.com/pve-docs/images/default-network-setup-routed.svg">https://pve.proxmox.com/pve-docs/images/default-network-setup-routed.svg</a></p></figcaption></figure>



### NAT e IPTABLES

Antes de continuar hagamos una breve parada. En lo poco que he dicho anteriormente ya salieron dos términos que pudieran llamar la atención y le dan título a esta sección: NAT e IPTABLES.

En Cisco packet tracer vimos como implementar NAT en un router para que un equipo que está en una red interna tenga salida a Internet haciendo una "traducción" de la IP interna a la IP externa o pública.&#x20;

<figure><img src="../../.gitbook/assets/image (381).png" alt=""><figcaption></figcaption></figure>

La misma idea es la que vamos a configurar aquí pero esta vez utilizando Linux e IPTables, pero refresquemos algunos conceptos:

**NAT -** es un protocolo de red utilizado para modificar las direcciones IP privadas en públicas en los encabezados de los paquetes cuando  pasan a través de un router o un firewall. Su propósito principal es permitir que varios dispositivos en una red local privada compartan una sola dirección IP pública para conectarse a Internet. Por tanto, lo que hace NAT es coger una dirección IP privada y traducirla a una dirección IP pública o viceversa.  Si quieres leer algo más sobre NAT ve a la sección de [NAT](../../redes/direccionamiento-ip/nat.md).

**IPTables -** es una herramienta de Linux que permite el filtrado de los paquetes de red, determinando qué paquetes de datos permitimos que lleguen hasta el servidor y cuáles no. Es una herramienta  necesaria que facilita la administración de firewalls en sistemas Linux. Como otros firewall,  funciona a través de reglas. Si quieres saber algo más del funcionamiento de iptables ve a la sección de [IPTables](../../miscelaneas/iptables.md).

¡Vamos a comenzar!

### Configurando una red interna en Proxmox

Lo primero es tener bien claro qué es lo que queremos hacer:&#x20;

* Instalaremos dos VMs de Ubuntu: una servirá de **router** y la otra de **cliente** en nuestra red interna
* El **cliente** estará conectada al linux bridge **vmbr1** que es una red interna
* El **router** estará conectada a los dos linux bridges: vmbr0 (que tiene salida a Internet a través de la interfaz física) y el vmbr1 que está conectado a la red interna.
* Aplicaremos una regla **NAT** en IPTABLES para redirigir el tráfico del cliente hacia Internet a través del router.

La configuración inicial con la que estoy trabajando se muestra en el esquema siguiente.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>Diagrama de la red</p></figcaption></figure>

Recuerda que Proxmox es una VM en VirtualBox y las dos VM de Ubuntu se encuentran dentro de Proxmox. La tabla siguiente especifica las características iniciales de la infraestructura a montar:

| Proxmox                     | VM Ubuntu Router        | VM Ubuntu Cliente       |
| --------------------------- | ----------------------- | ----------------------- |
| IP (estática): 10.0.2.15/24 | IP (dhcp): 10.0.2.16/24 | IP (dhcp): 10.0.2.17/24 |
| IP gateway: 10.0.2.2        | IP gateway: 10.0.2.2    | IP gateway: 10.0.2.2    |
| Red: NAT                    | Red: vmbr0              | Red: vmbr0              |

La VM de Proxmox tiene una IP estática configurada durante el proceso de instalación. Dicha VM está conectada en red `NAT` y como he dicho anteriormente, es el único modo en que he podido instalar VM o contenedores de Linux (LXC) y que todos tengan una IP y salida a Internet.

### Paso 1: Nueva interfaz de red en Proxmox

Por defecto, tenemos el `linux bridge` virtual `vmbr0` que es el que se conecta a nuestra interfaz de red  física y que toma del router físico una IP por DHCP.  Como queremos crear una red interna, tenemos que añadir una nuevo `linux bridge`  `vmbr1` al que conectaremos el equipo cliente.  Para ello vamos al nodo `pve - network - add (Linux Bridge)`.

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption><p>Añadir un nuevo linux bridge: vmbr1</p></figcaption></figure>

Lo único que tendremos que configurar es la IP de la red y, en mi caso, he utilizado la 10.10.10.253/24. No es necesario asignar una IP de gateway. De hecho, el gateway solo debe estar especificado una vez aunque tengamos configuradas varias interfaces de red.

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt="" width="563"><figcaption><p>La IP del Proxmox en el nuevo linux bridge es: 10.10.10.253/24</p></figcaption></figure>

Una vez añadido el linux bridge vmbr1, nos debe quedar algo como lo siguiente:

<figure><img src="../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption><p>Los dos linux bridge: vmbr0 y vmbr1</p></figcaption></figure>



### Paso 2: vmbr0 y vmbr1

La VM router estará conectada a los dos dos linux bridges: vmbr0 y vmbr1 . Recordemos que será esta VM la que le dará Internet al equipo cliente.&#x20;

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>VM router conectada a los dos linnux bridge</p></figcaption></figure>

En el caso de la VM cliente debemos conectarla a vmbr1, aunque bastaría con editar el  linux bridge vmbr0 y modificarlo para que aparezca conectado al vmbr1 que es el switch de nuestra red interna.

### Paso 3: Configurar las VM que harán de Router y de Cliente

Pasemos a las máquinas. Realmente las dos VM: router y cliente han sido clonadas de una VM de Ubuntu que tengo como plantilla.&#x20;

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt="" width="401"><figcaption><p>Las dos VM Router y Cliente como clones "dependientes" de la plantilla "ubuse1"</p></figcaption></figure>



Dado que las VM router y cliente son clones de la VM de Ubuntu Server  (ubuse1) que tengo como plantilla, podríamos cambiarle el nombre de cada una. Para ello, iniciamos ambas VM y editamos el siguiente archivo en cada una:&#x20;

```
nano /etc/hostname
```

Escribimos el nombre que le corresponda: router o cliente y reiniciamos para que se implementen los cambios correspondientes.&#x20;

Una vez restablecidas las VM debemos ver algo como lo siguiente, en la `VM router`, donde todavía NO hemos configurado la `IP` de la interfaz de red que se conecta al `vmbr1`.

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt="" width="563"><figcaption><p>Configuración de red de la VM router. La interfaz ens18 es la que se conecta al vmbr0</p></figcaption></figure>

A la `VM cliente` si que debemos configurarle la IP en modo estático porque no tenemos ningún servidor de DHCP que le brinde la IP. Tendría que hacerlo la VM router pero tampoco se lo hemos configurado. Por tanto, tenemos que asignarle una IP estática en la red interna y para ello utilizaremos la: 10.10.10.2/24 en la interfaz de red que se habilita: **ens18** y el **gateway** que le asignamos será la IP del router: **10.10.10.1.**

<figure><img src="../../.gitbook/assets/image (7).png" alt="" width="563"><figcaption><p>Configuración de red de la VM cliente en la red interna con la interfaz ens18</p></figcaption></figure>

### Paso 4: Configurando la red interna

Todo el trabajo de redirigir el tráfico de datos lo tiene que hacer la VM que hace de router así que volvamos a ella.&#x20;

**VM router** - Lo primero es configurar la IP estática para la nueva interfaz de red que le hemos habilitado: en este caso es la **ens19** y le asignamos la IP 10.10.10.1/24.

<figure><img src="../../.gitbook/assets/image (9).png" alt="" width="485"><figcaption><p>Configuración de la red para la VM router en la interfaz de red ens19 conectada al vmbr1</p></figcaption></figure>

Todavía con esto no podemos hacer que el cliente tenga conexión a Internet.



### Paso 5: IPTables :smile:

Lo primero será habilitar el IP forwarding y para ello nos vamos a editar el archivo `/etc/sysctl.conf` y quitar el # en la línea: **`net.ipv4.ip_forward=1`**

```
nano /etc/sysctl.conf 
```

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption><p>/etc/sysctl.conf</p></figcaption></figure>

La herramienta que nos hará el trabajo final será precisamente el `IPTABLES`. Para ello, tendremos que instalarla primero:

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

&#x20;**-t nat:** especifica la tabla a modificar. En este caso, la tabla `NAT` que es la tabla que gestiona la traducción de direcciones de red y se utiliza sobre todo para modificar las direcciones IP en los paquetes que atraviesan el firewall.

**-A POSTROUTING**: Agrega `-A` una regla al final de la cadena especificada.

* `POSTROUTING` es la cadena dentro de la tabla `nat` que procesa los paquetes justo antes de que salgan de una interfaz de red. Las reglas en esta cadena se utilizan para modificar los paquetes después de que hayan sido enrutados.

**-o ens18**: Especifica la interfaz de salida.

**-j MASQUERADE**: Significa "saltar" (jump). Lo que hace es especificar el objetivo que debe ser alcanzado si el paquete coincide con la regla.

* `MASQUERADE` lo que hace es ocultar la dirección IP del origen de los paquetes que salen por una interfaz de red. Esto es útil para compartir una conexión a Internet entre varios dispositivos en una red privada. Utiliza la dirección IP de la interfaz de salida como la dirección IP de origen del paquete.

En sentido general, la regla de IPTABLEs se utiliza para configurar una regla de enmascaramiento en la tabla `nat`, en la cadena `POSTROUTING.` De este modo todos los paquetes que salgan a través de la interfaz `eth0`, reemplaza la IP de origen del paquete con la IP de la interfaz de salida.

Podemos comprobar que efectivamente está habilitada la regla en la tabla NAT:

<figure><img src="../../.gitbook/assets/image (380).png" alt="" width="522"><figcaption><p>Regla habilitada en iptables</p></figcaption></figure>

### Paso 6: Testeando la conexión

Este es el momento en que debemos testear la conexión de la VM cliente a Internet a través de la VM router. Si hacemos un ping a google:

```
ping google.com
ping amazon.es
```

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Ping desde la Vm cliente a google.com en Internet</p></figcaption></figure>

Como se puede ver ya tenemos salida desde el equipo cliente hacia Internet a través de la VM router.

¿Hemos acabado? Pues no.&#x20;

### Paso 7

Las reglas de iptables que vayamos creando se almacenan en memoria, y cada vez que reiniciemos el servidor, se perderían y tendríamos  que volver a crearlas. Para evitar que esto ocurra, tenemos dos opciones:

1. Hacemos una copia manual de las reglas establecidas con el comando:

```
sudo iptables-save
```

2. Instalamos un nuevo paquete:

```
sudo apt install iptables-persistent -y
```

Durante el proceso de instalación nos preguntará si queremos guardar las reglas de IPv4 existentes en el archivo /etc/iptables/rules.v4 y las de IPv6 en el archivo /etc/iptables/rules.v6:

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt="" width="476"><figcaption><p>Reglas iptables almacenadas</p></figcaption></figure>

Podemos ver como se almacenan las reglas haciendo un cat o more de los archivos en cuestión:

<pre><code><strong>cat /etc/iptables/rules.v4
</strong>car /etc/iptables/rules.v6
</code></pre>

Con esta opción le estamos indicando al sistema que almacene las reglas en el archivo `/etc/iptables/rules.v4`. La próxima vez que se inicie el sistema, el script de inicio de iptables volverá a cargar las reglas almacenadas en ese archivo.

Y con esto si tenemos almacenadas las reglas de iptables en la MV que hace de router y a partir de aquí ya podemos crear nuestra propia infraestructura.



### <mark style="color:red;">¿Y si instalamos Nginx en el equipo cliente, cómo podemos acceder desde afuera?</mark>

Para probar lo primero que he hecho ha sido instalar Nginx en el equipo cliente, y comprobar que está el servicio activo, esto es:

**En el cliente**:

```
sudo apt install nginx
sudo systemctl status nginx.service
```

**En el router**:

Vamos a activar una regla NAT en IPTABLES que permita el acceso por el puerto 80. Recordemos que en mi caso tengo lo siguiente:

* La IP pública del gateway es `10.0.2.2`.
* El Proxmox tiene la IP `10.0.2.15`.
* Tenemos como servidor web el cliente, en la red interna con la IP `10.10.10.16`.
* Queremos redirigir el tráfico que llega al puerto 80 - HTTP de la IP pública a la IP interna del servidor web en el mismo puerto 80.

Como ya creamos una regla NAT para permitir la salida a Internet del tráfico del equipo cliente, tenemos habilitado el reenvío de paquetes IP.

Recordemos que se trata de  editar el archivo `/etc/sysctl.conf` y descomentar la línea:

```bash
net.ipv4.ip_forward = 1
```

Y luego aplicar los cambios:

```bash
sudo sysctl -p
```

#### Creando la regla NAT de entrada (DNAT)

El siguiente comando de `iptables` permite redirigir el tráfico que llega al puerto 80 en la IP pública hacia el servidor en la red interna:

```bash
sudo iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j DNAT --to-destination 10.10.10.16:80
```

#### donde:

* `-t nat`: Indica que estamos modificando la tabla NAT.
* `-A PREROUTING`: Añade una regla en la cadena `PREROUTING`, que se encarga de redirigir el tráfico entrante antes de que llegue a cualquier proceso local.
* `-i ens18`: Indica que esta regla se aplica a la interfaz de red externa.
* `-p tcp`: Especifica que la regla aplica solo a paquetes TCP.
* `--dport 80`: Indica que la regla se aplica al tráfico entrante en el puerto 80.
* `-j DNAT`: Usamos la acción `DNAT` (Destination Network Address Translation) para redirigir el tráfico a otra IP.
* `--to-destination 10.10.10.16:80`: Redirige el tráfico al servidor interno `10.10.10.16`en el puerto 80.

#### 3. Configurarando la regla de reenvío (FORWARD)

Debes permitir el reenvío de paquetes desde la red externa a la interna. Añade la siguiente regla a la cadena `FORWARD`:

```bash
sudo iptables -A FORWARD -p tcp -d 10.10.10.16 --dport 80 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT
```

#### Guardamos las reglas

Para asegurarte de que las reglas de `iptables` persisten tras un reinicio, debes guardarlas. Dependiendo de la distribución de Linux, puedes usar:

```bash
sudo netfilter-persistent save
```

<mark style="color:red;">Probemos!!</mark>

Funciona, sin embargo he tenido que activar el reenvío de puertos en el adaptador NAT que conecta al Proxmox:

<figure><img src="../../.gitbook/assets/image (386).png" alt=""><figcaption><p>Reenvío de puertos en el adaptador "NAT"</p></figcaption></figure>

### Links

* [https://www.youtube.com/watch?v=sGdhakDeQyo](https://www.youtube.com/watch?v=sGdhakDeQyo) (excelente video)
* OpenWebinars: Proxmox VE: Redes
* [https://coda.io/@julia-asensio-pedrero/proxmox/introduccion-a-las-redes-en-proxmox-22](https://coda.io/@julia-asensio-pedrero/proxmox/introduccion-a-las-redes-en-proxmox-22)
* [https://www.speaknetworks.com/enable-intel-vt-amd-v-support-hardware-accelerated-kvm-virtualization-extensions/](https://www.speaknetworks.com/enable-intel-vt-amd-v-support-hardware-accelerated-kvm-virtualization-extensions/)
* [https://www.debian.org/doc/manuals/debian-reference/ch05.es.html](https://www.debian.org/doc/manuals/debian-reference/ch05.es.html)
* [https://millaredos.com/proxmox-configurar-internet-una-sola-interfaz-de-red/](https://millaredos.com/proxmox-configurar-internet-una-sola-interfaz-de-red/)
* [https://help.ovhcloud.com/csm/es-es-dedicated-servers-firewall-iptables?id=kb\_article\_view\&sysparm\_article=KB0043439](https://help.ovhcloud.com/csm/es-es-dedicated-servers-firewall-iptables?id=kb\_article\_view\&sysparm\_article=KB0043439)

