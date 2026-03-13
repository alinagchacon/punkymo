---
description: Instalación
---

# GNS3 en Windows

### Instalación en Windows

Para instalar, configurar y usar GNS3 tenemos que descargar GNS3 para Windows del sitio oficial [https://www.gns3.com/software/download](https://www.gns3.com/software/download).

Una vez descargado e instalado GNS3 en nuestro PC, nos toca  seleccionar la manera en la que vamos a trabajar con GNS3, esto, tenemos que definir cómo levantar el **Main Server**. Para ello tenemos tres modos:\
• Escritorio\
• Máquina virtual\
• Web

Lo recomendable es trabajar con la VM de GNS3, porque así tendremos controlados los recursos como la RAM, la CPU y espacio en disco, que se usan y hacerlo de un modo más eficiente.

Lo más eficiente sería trabajar con VMWare, pero como tenemos VirtualBox instalado en nuestros equipos del centro, pues ese es el hypervisor que usaremos.&#x20;

Cuando tengamos la VM de GNS3 descargada la importamos en Virtualbox y configuramos las características de la misma, aunque me ha funcionado bien dejándolo todo por defecto:

Podemos asignar los recursos que necesitemos, por ejemplo:\
• 6 GB de RAM\
• 5 procesadores\
• 1 adaptador de Red en modo Bridge.

Ahora nos vamos al GNS3 local que hemos instalado y una vez dentro abrimos **Edit - Preferences -Server** y deshabilitamos **Enable local server**. Podemos especificar: Protocol, Host, Port, Auth, User y Password.

<figure><img src="../../.gitbook/assets/image (447).png" alt="" width="563"><figcaption><p>Tomado del pdf de Nicolás Zábala</p></figcaption></figure>

Si lo hemos configurado todo bien, nos debe aparecer en la columna de la derecha  **Servers Summary** el **Main Server** en verde.

<figure><img src="../../.gitbook/assets/image (448).png" alt="" width="372"><figcaption><p>Tomado de lps apuntes de Nicolás Zábala</p></figcaption></figure>

{% hint style="info" %}
Nota: Podemos fijar la IP desde el menú principal en el Main Server y seleccionar la opción “Network”.&#x20;
{% endhint %}

### Importando imágenes de dispositivos en GNS3

Para importar una imagen de un dispositivo en **GNS3**, tenemos que utilizar la opción **New Appliance** (Nuevo dispositivo) que es la opción que nos permite **añadir un equipo virtual** al proyecto que **no viene incluido por defecto**.

**Pero, ¿qué es exactamente un&#x20;**_**appliance?**_

Un _appliance_ en GNS3 es como una **plantilla de dispositivo de red** (router, switch, firewall, servidor, etc.) ya preparada para funcionar en el simulador, y normalmente incluye:

* El **tipo de máquina** (QEMU, VirtualBox, VMware, Docker, Dynamips…)
* La **imagen del sistema** (IOS, ISO, qcow2, etc.)
* Las **Interfaces de red**
* Los parámetros básicos de CPU, RAM y almacenamiento.

El appliance sirve para:

* Importar **dispositivos que no están** en la instalación base o inicial. Realmente apenas trae nada.
* Añadir **equipos reales virtualizados**, como por ejemplo:
  * Routers Cisco (IOSv, IOSvL2)
  * Firewalls (pfSense, OPNsense)
  * Servidores Linux
  * Equipos de seguridad (Kali, Security Onion)
* Usar **appliances oficiales** del repositorio de GNS3 o crear uno personalizado



#### Añadiendo un router

Entonces, volviendo a lo nuestro vamos a buscar la opción New template para añadir la imagen de un Router Cisco de la serie 7000, específicamente&#x20;

<figure><img src="../../.gitbook/assets/image (449).png" alt="" width="375"><figcaption></figcaption></figure>

Y seguimos las instrucciones. Un punto importante es que la imagen que ahora vamos a añadir es la de Cisco 7200 124-24-TS, por tanto, tenemos que buscar exactamente esa appliance para poder importarla.

<figure><img src="../../.gitbook/assets/image (451).png" alt="" width="563"><figcaption></figcaption></figure>

Una vez seleccionada la imagen, le damos importar y terminamos el proceso con un nuevo router instalado en GNS3.

### Importando imágenes de CISCO IOU en GNS3

Para importar los switches virtuales de Cisco utilizaremos las imágenes del siguiente repositorio:

[https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG?tab=readme-ov-file](https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG?tab=readme-ov-file)

#### Añadiendo switches de capa 2 y capa 3 de OSI

Aquí tenemos imágenes de Cisco de capa 2 y capa 3. Las imágenes de capa 3 vienen con funciones de Routing: OSPF, EIGRP, así como switching VLAN, spanning tree,  etc. En cambio, las de capa 2\
solo switching. Importaremos a nuestro servidor GNS3 las siguientes imágenes enmarcadas en rojo, esto es, un switch de capa 2 y otro de capa 3:

<figure><img src="../../.gitbook/assets/image (452).png" alt="" width="563"><figcaption><p>Tomado de la documentación de Nicolás Zábala</p></figcaption></figure>

Nos vamos a E**dit - Preferences - IOU Devices** y nos pregunta por el tipo de imagen que es. En este caso seleccionamos **L3**, porque es de capa 3.

<figure><img src="../../.gitbook/assets/image (453).png" alt="" width="563"><figcaption></figcaption></figure>

Es cuestión de seguir las instrucciones y poco más. Nos debe aparecer así:

<figure><img src="../../.gitbook/assets/image (454).png" alt="" width="377"><figcaption></figcaption></figure>



Para el tema de la licencia ver:

* [https://www.ipvanquish.com/download/CiscoIOUKeygen3f.py](https://www.ipvanquish.com/download/CiscoIOUKeygen3f.pyhttps://www.ipvanquish.com/download/)
* [https://www.ipvanquish.com/download/](https://www.ipvanquish.com/download/CiscoIOUKeygen3f.pyhttps://www.ipvanquish.com/download/)

### Añadiendo un firewall de Cisco ASA

Vamos a incorporar a nuestro GNS3 un firewall de Cisco ASA. Para ello nos volvemos al github: [https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG?tab=readme-ov-file](https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG?tab=readme-ov-file) y descargaremos el 3, o sea, **asav992-32-qcow2**.

<figure><img src="../../.gitbook/assets/image (455).png" alt="" width="563"><figcaption><p>De <a href="https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG?tab=readme-ov-file">https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG?tab=readme-ov-file</a></p></figcaption></figure>

Seguimos los pasos por defecto y añadimos el firewall.



### Conectarnos via Putty al Main Server

Si queremos conectarnos via Putty al Main Server debemos hacer lo siguiente:

```
sudo su
sudo apt-get update
sudo apt-get install python3
wget http://www.ipvanquish.com/download/CiscoIOUKeygen3f.py
python3 CiscoIOUKeygen3f.py
```

Esto nos mostrará la siguiente información:

<figure><img src="../../.gitbook/assets/image (456).png" alt="" width="563"><figcaption><p>Tomado del documento de Nicolás Zábala</p></figcaption></figure>

Nos vamos a **GNS3 - Edit - Preferences - IOS** on UNIX preferences y copiamos la información de licencia.

<figure><img src="../../.gitbook/assets/image (457).png" alt="" width="563"><figcaption><p>Tomado del documento de Nicolás Zábala</p></figcaption></figure>



### Una topología sencilla

La siguiente topología muestra un switch conectado a la red con dos PC.&#x20;

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt="" width="563"><figcaption><p>Una topología sencilla con acceso a Internet</p></figcaption></figure>

Y la siguiente imagen nos muestra como estoy conectada desde mi terminal al PC2 para configurarlo.

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt="" width="563"><figcaption><p>Conexión desde  mi terminal al PC2 de GNS3</p></figcaption></figure>



En **GNS3**, el dispositivo **NAT** tiene como función **dar acceso a Internet a los equipos del laboratorio** sin necesidad de configurar la red física del equipo anfitrión.

Este **NAT de GNS3** actúa como un **router con traducción de direcciones (Network Address Translation)** entre la **red virtual** del proyecto y la **red real** del sistema anfitrión que en mi caso es un Debian. Por tanto,

* Traduce las **IP privadas** de los dispositivos del laboratorio
* Las convierte en la **IP real del host**
* Permite que los nodos virtuales **salgan a Internet**
* Impide, por defecto, accesos desde Internet hacia dentro del laboratorio



El NAT de GNS3 se utiliza principalmente para:

* Descargar paquetes y actualizaciones desde los nodos virtuales
* Instalar software en máquinas Linux del laboratorio
* Permitir pruebas de conectividad (ping, curl, apt, etc.)
* Simular un **acceso a Internet básico** y seguro

Es ideal para **prácticas educativas**, ya que no requiere configuraciones complejas ni cambios en la red real.

### ¿Cómo funciona a nivel sencillo?

```
[PC Debian real]
        |
     (NAT GNS3)
        |
[Red virtual GNS3]
        |
[Router / PC / Firewall]
```

* Los dispositivos usan **IP privadas**
* El NAT hace la traducción al salir
* El tráfico vuelve correctamente al laboratorio

{% hint style="info" %}
Nota: Para escenarios más realistas se suele usar **Cloud**, **bridge** o **routers propios**.
{% endhint %}

## Links

* Instalación en Windows
  * [https://docs.gns3.com/docs/getting-started/installation/windows/](https://docs.gns3.com/docs/getting-started/installation/windows/)
  * [https://ccnadesdecero.es/como-instalar-gns3-windows](https://ccnadesdecero.es/como-instalar-gns3-windows/)
* Cisco - imágenes de dispositivos
  * [https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG?tab=readme-ov-file](https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG?tab=readme-ov-file)
  * [https://ccnadesdecero.es/descargar-cisco-ios-gns3/](https://ccnadesdecero.es/descargar-cisco-ios-gns3/)
  * [https://drive.google.com/drive/folders/102jxZ9ECpe6ZFtXYdK\_81iEVuuFoGOGR](https://drive.google.com/drive/folders/102jxZ9ECpe6ZFtXYdK_81iEVuuFoGOGR)
  * [https://gns3.com/marketplace/appliances](https://gns3.com/marketplace/appliances)
  * [https://wiki.opnsense.org/manual/install.html](https://wiki.opnsense.org/manual/install.html)
  * [https://www.youtube.com/watch?v=47HWtHtbjxY](https://www.youtube.com/watch?v=47HWtHtbjxY)
  * [https://www.youtube.com/watch?v=yqhrAs58y5c\&t=85s](https://www.youtube.com/watch?v=yqhrAs58y5c\&t=85s) - qemu
  * https://www.youtube.com/watch?v=Ibe3hgP8gCA\&list=PLhfrWIlLOoKNFP\_e5xcx5e2GDJIgk3ep6 \*
  * [https://www.youtube.com/watch?v=2aI6ipuJPLo](https://www.youtube.com/watch?v=2aI6ipuJPLo) \*
  * [https://www.youtube.com/watch?v=tBswpi22q\_0](https://www.youtube.com/watch?v=tBswpi22q_0) \*\*
  * [https://www.youtube.com/watch?v=W2benl0R-A4](https://www.youtube.com/watch?v=W2benl0R-A4) \*\*
* Instalación en Linux y específicamente en Debian 12
  * [https://www.javiercd.es/posts/redes/instalar\_gns3\_debian12/instalar\_gns3\_debian12](https://www.javiercd.es/posts/redes/instalar_gns3_debian12/instalar_gns3_debian12)
  * [https://docs.gns3.com/docs/getting-started/installation/linux](https://docs.gns3.com/docs/getting-started/installation/linux)
* Cursos - training
  * [https://gns3.teachable.com/l/products?sortKey=name\&sortDirection=asc\&page=1](https://gns3.teachable.com/l/products?sortKey=name\&sortDirection=asc\&page=1)
