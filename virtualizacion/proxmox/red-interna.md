---
description: Proxmox
---

# Red interna

_En duda y analizando .... recuerda que yo también estoy aprendiendo._

He estado testeando Proxmox en diferentes condiciones pero siempre las mínimas. Me refiero a tener una única interfaz de red. Aún en esta situación cualquier VM que creara dentro de Proxmox debería tener conexión con el linux bridge `vmbr0` pero no lo conseguía.

De hecho, cualquier sistema operativo que instalaba se bloquea en el punto de buscar el mirror para acabar de descargar los paquetes. Ese bloqueo se debía a que no existía conexión con Internet.

Por otra parte, seguía teniendo un fallo con la `virtualización por hardware` en Proxmox.  Esto no tiene nada que ver con tener o no activada la virtualización a nivel de la BIOS.&#x20;

Más bien se basa en tecnologías de virtualización asistida por hardware como KVM para permitir la creación y ejecución eficiente de VM en servidores físicos, proporcionando mejor rendimiento y seguridad en entornos de virtualización.&#x20;

Se aprovechan al máximo las características de virtualización de hardware de los procesadores modernos para crear y ejecutar máquinas virtuales de un modo más eficiente y con mayor rendimiento.  Algunas de las ventajas de esto son:

**Rendimiento**: Las VM aprovechan el poder de procesamiento de la CPU y accedeny consiguen un mejor rendimiento.

**Seguridad**: Mejora la seguridad  y reduce las posibilidades de ataques entre máquinas virtuales.

**Aislamiento**: Proporciona alto grado de aislamiento entre las VM, lo que significa que una VM no puede acceder ni afectar directamente a otras en el mismo host.



Para asegurarnos de habilitar la virtualización por hardware puede que nos ayude la siguiente guía: [https://www.geeknetic.es/Guia/1873/VirtualBox-Como-activar-la-virtualizacion-por-hardware.html](https://www.geeknetic.es/Guia/1873/VirtualBox-Como-activar-la-virtualizacion-por-hardware.html)&#x20;

A grandes rasgos se trata de verificar si lo tenemos habilitado o no. Para ello nos vamos al Administrador de tareas, a la pestaña de la CPU y buscamos el estado de la virtualización del equipo.

Si la virtualización está habilitada con Hyper-V tendremos que deshabilitar el Hyper-V si queremos virtualizar sistemas de 64bits o no podríamos crear VM de 64bits con VirtualVBox.

Para deshabilitar Hyper-V nos vamos a Activar a desactivar características de Windows, buscamos Hyper-V y lo desmarcamos la opción habilitada.&#x20;

Si lo tenemos habilitado entonces, podemos entrar a la configuración de la VM creada y buscar el apartado de Sistema - Aceleración y podemos ver el tipo de virtualización habilitada en el desplegable de paravirtualización.

Algunas de las siguientes opciones se encuentran entre las características a seleccionar:

**Ninguno**: VirtualBox no aprovecha ningún tipo de tecnología de virtualización.

**Predeterminado**: habilitado automáticamente al crear la VM. Utiliza el método más adecuado en función del S.O. que hayamos seleccionado. La recomendable por defecto.

**KVM**: interfaz de hipervisor para Linux a partir de la versión 2.6.25 del Linux Kernel. Cualquier S.O. que haga uso de esa versión o cualquier versión posterior del Linux Kernel, deberá utilizar este modo de paravirtualización. Iigualmente, se selecciona automáticamente con la configuración por defecto.

\


<figure><img src="../../.gitbook/assets/image (237).png" alt=""><figcaption><p>Pestaña de virtualización en una VM d eVirtualBox</p></figcaption></figure>

En VMware tenemos:

<figure><img src="../../.gitbook/assets/image (238).png" alt=""><figcaption><p>Virtualización en VMware</p></figcaption></figure>

Digamos que es fundamental tener bien configurado nuestro VirtualBox o el VMware player si queremos que nuestro hipervisor de Proxmox funcione correctamente. De hecho, antes de corregir el fallo al instalar Proxmox me salía el siguiente mensaje:

<figure><img src="../../.gitbook/assets/image (239).png" alt=""><figcaption><p>Mensaje de error en el proceso de instalación de Proxmox</p></figcaption></figure>

Incluso, aunque terminaba instalando Proxmox y pudiendo crear una VM dentro, al intentar el proceso de instalación me muestra:

<figure><img src="../../.gitbook/assets/image (240).png" alt=""><figcaption><p>Error al levantar una VM en Proxmox</p></figcaption></figure>

Y es evidente que la solución no pasa por deshabilitar la virtualización por hardware.

### Red Interna

Ahora si me dispongo a analizar la configuración de una red interna dentro de Proxmox. Se trata de utilizar una única interfaz de red para tener acceso a Internet y una red interna para varias VM en Proxmox.

Para ello, seguí la guía de Proxmox en: [https://pve.proxmox.com/wiki/Network\_Configuration](https://pve.proxmox.com/wiki/Network\_Configuration)&#x20;



<figure><img src="../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

En esta guía que os recomiendo, una de las opciones es el enmascaramiento (NAT) con iptables. Este enmascaramiento nos permite el acceso a la red utilizando la dirección IP del host para el tráfico saliente, teniendo una dirección IP privada, como es el caso.

Utilizamos iptables para reescribir cada paquete saliente de modo que parezca que se origina en el host. Naturalmente, las respuestas se reescriben para enrutarse al remitente original.

De hecho, la guía ha sido de la misma wiki de Proxmox:

&#x20;

<figure><img src="../../.gitbook/assets/image (242).png" alt=""><figcaption><p><a href="https://pve.proxmox.com/pve-docs/images/default-network-setup-routed.svg">https://pve.proxmox.com/pve-docs/images/default-network-setup-routed.svg</a></p></figcaption></figure>



La configuración que monté fue la siguiente:

En Proxmox:

* RAM: 6GB / Procesadores: 8
* Red: en modo puente (bridge)

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Configuración de la VM de Proxmox en VMware</p></figcaption></figure>

Las direcciones IP de Proxmox serían:

* **ens33**: 192.168.1.128 / gateway: 192.168.1.1 (en la misma red del host con IP: 192.168.1.100)
* **vmbr0**: 192.168.44.128 (el linux bridge)

Las IP se configuran de manera estática como muestra la siguiente imagen:

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Configuración de /etc/network/interfaces en Proxmox</p></figcaption></figure>

Adicionalmente, añadimos las líneas correspondientes al IPTABLES para reescribir los paquetes de entrada y salida, de modo que parezca que se originan en el host. También tenemos que descomentar la línea `net.ipv4.ip_forward=1` en el fichero `/etc/sysctl.conf`:

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Archivo /etc/sysctl.conf</p></figcaption></figure>

Por supuesto, tenemos que actualizar el servicio de networking para que asuma los cambios realizados. Para ello hacemos:

```
systemctl restart networking.service
```



No fue lo único. Como ya tenía instalada una VM con Debian tuve que modificar manualmente la dirección IP para que estuviera en la red interna de vmbr0. Esto es: 192.168.44.130/24 y el gateway  192.168.44.128 que se corresponde con la IP del host.&#x20;

Adicionalmente, tuve que editar el  `/etc/apt/sources.list` para poder actualizar la lista de paquetes una vez que tenía conexión con el exterior a través del host.

Recuerda que el archivo `sources.list` es un componente esencial en los sistemas basados en Debian, como Ubuntu. Permite gestionar los repositorios de software desde los cuales el S.O.  descarga e instala paquetes y actualizaciones.&#x20;

Este archivo contiene la lista de URLs que apuntan a los repositorios de paquetes, donde cada línea corresponde con un repositorio que contiene información de la ubicación, versión entre otros detalles.&#x20;

Os dejo los enlaces que me han ayudado.

### Links

1. [https://www.speaknetworks.com/enable-intel-vt-amd-v-support-hardware-accelerated-kvm-virtualization-extensions/](https://www.speaknetworks.com/enable-intel-vt-amd-v-support-hardware-accelerated-kvm-virtualization-extensions/)
2. [https://www.debian.org/doc/manuals/debian-reference/ch05.es.html](https://www.debian.org/doc/manuals/debian-reference/ch05.es.html)
3. [https://millaredos.com/proxmox-configurar-internet-una-sola-interfaz-de-red/](https://millaredos.com/proxmox-configurar-internet-una-sola-interfaz-de-red/)

&#x20;

