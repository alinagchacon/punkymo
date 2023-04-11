# Entorno Proxmox

Proxmox aplica el concepto de datacenter donde se encuentran los nodos que son los sistemas de virtualización. Dentro del nodo tenemos un storage local que contiene los discos de las máquinas virtuales, backup, isos y templates.

Dentro del propio sistema operativo los podemos encontrar almacenados en /var/lib/vz/. Dentro de este directorio encontramos:

* /var/lib/vz/dump – guarda los backups de las VM.
* /var/lib/vz/templates - guarda las VM que permiten generar sistemas virtuales.
* /var/lib/vz/images/ - guarda los discos de las VM.
* /var/lib/vz/dump/iso - almacenan las ISO.

## Las máquina virtual

### Crear una VM

Para crear una VM nos ubicamos sobre el nodo “pve”:

* Cada VM se crea con un número asociado, comenzando por el 100. El nombre lo ponemos nosotros.
* A continuación, seleccionamos el sistema operativo que vamos a instalar. &#x20;
* Indicamos dónde se encuentra la ISO del sistema a instalar.
* Elegir el storage donde se encuentra la imagen.
* Seleccionamos el tipo de disco que vamos a utilizar para crear la VM. El disco puede ser IDE, SATA o VIRTIO. Los discos VIRTIO son mucho más rápidos que los IDE o los SATA.
* También podemos elegir en qué storage vamos a colocar la imagen. En este caso el storage es local (no tenemos más), luego indicamos el tamaño y el tipo de filesystem.\
  Podemos usar el formato qcow2 que reduce el consumo de datos usando thin provisioning. Además, podemos usar el formato raw (estático), o de disco vmware.

En el tema de drivers: realteck, E1000 y VIRTIO. A tener en cuenta que los Windows viejos, como XP, prefieren realteck o E1000.

Proxmox crea una interface vmbr0 para asociarla a la VM, luego crea un bridge con el dispositivo físico del servidor Proxmox. Las configuraciones de los distintos bridges e interfaces de Proxmox se configurarn desde el archivo: `/etc/network/interfaces`donde vemos que el bridge vmbr0 está asociado con el bridge\_ports enp0s3 que es la interface que configuramos durante la instalación.

### &#x20;Arrancar la VM

Una vez creada la VM vamos a arrancarla. Esto lo podemos hacer seleccionando la máquina virtual, luego con el click derecho seleccionaremos la opción Start. Las opciones que aparecen en el cuadro son las siguientes

* Start: arrancar la máquina virtual.
* Migrar: podemos migrar la máquina virtual a otro nodo del cluster.
* Resume: hacer un snap de la virtual corriendo en ese momento.
* Stop: apagar la virtuales.
* Clone: duplicar la virtuales.
* Convert to Template: máquinas virtuales llevadas a plantillas.
* Console: abrir la terminal openvnc o spice para trabajar con la virtual.

### Backup

Esta pestaña es importante porque nos permite crear backups de la VM corriendo sin necesidad de apagarla.

Para hacerlo nos ubicamos sobre la máquina virtual y hacemos click en Backup Now.

Proxmox nos propone un cuadro de diálogo para saber en qué storage tiene que hacer el backup y también nos pregunta si queremos comprimir o no el backup.

### &#x20;Snapshots

Podemos crear instantáneas de la VM estando en ejecución. Sobre todo, si estamos en un punto crítico de configuración de un servicio.

### Console

Nos permite acceder a la VM sin necesidad de salir del servidor.

### &#x20;Internet

Cada VM puede tener varias interfaces de red, de cuatro tipos diferentes:

* **Intel E1000** - predeterminado y emula una tarjeta de red Intel Gigabit.
* **VirtIO** - NIC paravirtualizada que debe usarse si desea obtener el máximo rendimiento. Como todos los dispositivos VirtIO, debemos tener instalado el controlador adecuado.
* **Realtek 8139 -** emula una tarjeta de red de 100 MB/s antigua y debe usarse cuando se tienen S.O más antiguos (antes de 2002)
* **Vmxnet3** - es otro dispositivo paravirtualizado, que solo debe usarse al importar una VM desde otro hipervisor.

Proxmox VE generará para cada NIC una **dirección MAC** aleatoria, de modo que su VM sea direccionable en redes Ethernet.

La NIC agregada a la máquina virtual puede seguir uno de dos modelos diferentes:

* en el modo puente predeterminado, cada NIC virtual está respaldado en el host por un _dispositivo tap_ (un dispositivo de bucle invertido de software que simula una NIC Ethernet). Este dispositivo tap se agrega a un puente, por defecto vmbr0 en Proxmox VE. En este modo, las máquinas virtuales tienen acceso directo a la LAN Ethernet en la que se encuentra el host.
* en el modo NAT alternativo, cada NIC virtual solo se comunicará con la pila de red del usuario de Qemu, donde un enrutador integrado y un servidor DHCP pueden proporcionar acceso a la red. Este DHCP incorporado servirá direcciones en el rango privado 10.0.2.0/24. El modo NAT es mucho más lento que el modo puenteado y solo debe usarse para realizar pruebas. Este modo solo está disponible a través de CLI o API, pero no a través de WebUI.



### Default Configuration using a Bridge

Los bridges son switches de red físicos implementados en software. Todos los guest virtuales pueden compartir un solo bridge, o puede crear varios para separar los dominios de la red. Cada host puede tener hasta 4094 puentes.&#x20;

<figure><img src="../../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

El programa de instalación crea un único puente denominado vmbr0, que se conecta a la primera tarjeta Ethernet. La configuración correspondiente en `/etc/network/interfaces` podría verse así:

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption><p>configuración de red</p></figcaption></figure>

### Routed Configuration

Se puede enrutar todo el tráfico a través de una sola interfaz, lo que asegura que todos los paquetes de red usen la misma dirección MAC.&#x20;

Un escenario común es que tiene una IP pública, supongamos que la 198.51.100.5  y un bloque de IP adicional para las VM 203.0.113.16/28. Entonces Se recomienda la siguiente configuración:

```
auto lo
iface lo inet loopback
 
auto eno0
iface eno0 inet static
        address  198.51.100.5/29
        gateway  198.51.100.1
        post-up echo 1 > /proc/sys/net/ipv4/ip_forward
        post-up echo 1 > /proc/sys/net/ipv4/conf/eno0/proxy_arp
 
auto vmbr0
iface vmbr0 inet static
        address  203.0.113.17/28
        bridge-ports none
        bridge-stp off
        bridge-fd 0
```

### Tipos de redes

**Linux Bond** – (channel bonding o unión de interfaces de red) no es más que la suma de dos o más interfaces de red para aumentar el ancho de banda o la redundancia.\
\
Implica la división del tráfico de red entre las diferentes interfaces de red físicas. Lo que hace es simular un dispositivo de red con gran ancho de banda uniendo varias tarjetas de red independientes, de manera que las aplicaciones sólo verán un interfaz de red.

**Linux VLAN** - &#x20;

**Linux Bridge** – es un dispositivo que une dos o más segmentos de red de forma transparente, por lo que un bridge es independiente de cualquier protocolo de red o transporte porque actúa a nivel de capa 2 de OSI.

**Open vSwitch (OVS)** – es un software de código abierto, diseñado para ser utilizado como un switch virtual en entornos de servidores virtualizados. Es el encargado de reenviar el tráfico entre diferentes VM en el mismo host físico y de reenviar el tráfico entre las VM y la red física.

Se instala:&#x20;

```
apt-get install openvswitch-switch
```

**OVS Bridge** – es un ambiente de redes en un dispositivo que une dos o más segmentos de red de forma transparente.

**OVS Bond** – es la suma de dos o más interfaces de red para aumentar el ancho de banda o la redundancia. Implica la división del tráfico de red entre las distintas interfaces de red físicas.

**OVS InPort** – para que el host (el propio proxmox) utilice una vlan dentro del puente, debe crear este tipo de interfaces. Estos dividen una interfaz virtual en la vlan especificado a la que puede asignar una dirección IP o usar DHCP. Tienen que aparecer en la definición de puente real en ovs\_ports. De no ser así, no se muestran a pesar de haber especificado un ovs\_bridge.



<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>





