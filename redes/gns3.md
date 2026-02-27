# GNS3

## Introducción

¿Qué es GNS3?

¿Para qué se utiliza en el ámbito de las redes y la ciberseguridad?

¿Qué ventajas ofrece frente a otros simuladores o laboratorios físicos de redes?

¿Cómo se integra con herramientas externas como VirtualBox, VMWare o Docker y qué beneficios aporta esta integración?

### Instalación en Windows

<mark style="color:purple;">Falta....</mark>



### Instalación en Debian

Actualizamos los repositorios e instalamos las actualizaciones disponibles del sistema.

```
sudo apt update -y && sudo apt upgrade -y
```

Instalamos las dependencias necesarias para GNS3, incluyendo Python, herramientas de virtualización (KVM, QEMU, libvirt), bibliotecas adicionales (PyQt5, Dynamips)

```
sudo apt install python3 python3-pip pipx python3-pyqt5 python3-pyqt5.qtwebsockets python3-pyqt5.qtsvg qemu-kvm qemu-utils libvirt-clients libvirt-daemon-system virtinst dynamips software-properties-common ca-certificates curl gnupg2 bridge-utils virt-manager libvirt-daemon -y
```

Habilitamos y arrancamos el servicio de virtualización libvirtd y nos añadimos al grupo “libvirt” para otorgarle permisos sobre las máquinas virtuales y redes de KVM .

```
sudo systemctl enable --now libvirtd && sudo usermod -aG libvirt $(whoami)
```

Instalamos las aplicaciones principales de GNS3, esto es:&#x20;

* el servidor - gns3-server&#x20;
* la interfaz gráfica - gns3-gui&#x20;

Utilizamos `pipx`  (herramienta de Python) para instalar y ejecutar aplicaciones de Python empaquetadas como scripts ejecutables (CLI):&#x20;

```
pipx install gns3-server && pipx install gns3-gui
```

Aseguramos que el directorio de binarios de pipx esté en la variable de entorno PATH para que los comandos de GNS3 sean accesibles globalmente desde cualquier terminal.

```bash
pipx ensurepath
```

Insertamos el paquete `PyQt5` en el entorno de gns3-gui, para asegurarnos que tenga todas las dependencias necesarias. Este paquete es un conjunto de enlaces de Python para la biblioteca `Qt`, que es un framework multiplataforma para el desarrollo de interfaces gráficas de usuario (GUI).

```
pipx inject gns3-gui gns3-server PyQt5
```

Iniciamos la red por defecto de KVM y la configuraremos para que se inicie de manera automática.

```
virsh --connect=qemu:///system net-start default
virsh --connect=qemu:///system net-autostart default
```

Reiniciamos el sistema para aplicar los cambios, como la configuración de grupos y PATH.

```
sudo reboot 
```

Ahora si estamos en condiciones de  iniciar gns3:

```
gns3 &
```

## Ubridge

Se trata de un componente esencial de GNS3 que se ejecuta como `root` para permitir la inyección de paquetes en la interfaz de red. Cuando se ejecuta como root, representa  un riesgo de escalada de privilegios. En Linux y Windows, el acceso a la red es limitado, pero en OS X, ubridge tiene acceso total.&#x20;

Este componente permite interconectar redes del entorno virtual de GNS3 con redes externas o con otros tipos de nodos dentro del mismo proyecto, especialmente cuando estamos usando contenedores, máquinas virtuales o dispositivos que no están en la misma red interna. `ubridge` permite:

* Conectar interfaces del host con GNS3.
*   Enlazar redes entre diferentes tipos de nodos, como:

    * dispositivos emulados por QEMU/KVM
    * contenedores Docker
    * máquinas virtuales
    * switches genéricos
    * y conexiones a la red física.



### Instalación

Primero vamos a clonar el repositorio de ubridge desde GitHub

```
git clone https://github.com/GNS3/ubridge.git
```

Nos dirigimos al directorio donde tenemos el proyecto recién clonado

```
cd ubridge/
```

Y compilamos el código fuente e instalamos el binario en el sistema

```
make 
sudo make install
```

Ahora asignamos permisos de ejecución al archivo binario de ubridge

```
chmod +x ubridge
```

Copiamos el archivo binario a “/usr/local/bin”,  para que esté disponible de modo global en el sistema

```
cp -p ubridge /usr/local/bin
```

Configuramos permisos especiales para ubridge, permitiéndole acceso a operaciones de red (cap\_net\_admin) y envío de paquetes en bruto (cap\_net\_raw), necesarias para su funcionamiento.

```
sudo setcap cap_net_admin,cap_net_raw=ep /usr/local/bin/ubridge
```



En entorno virtual&#x20;

En nuestro caso haremos la instalación de GNS3 como VM en VMWare Workstation. Para ello, nos descargamos la OVA correspondiente desde la página oficial:

<figure><img src="https://github.com/user-attachments/assets/fbb5fcff-b3e4-4079-a6d9-1b2a5af33c47" alt="" width="375"><figcaption></figcaption></figure>

Y una vez tengamos la OVA, la importamos como una VM:

&#x20;

<figure><img src="https://github.com/user-attachments/assets/a3501b30-47c9-4429-9191-75fab70daebd" alt="" width="375"><figcaption></figcaption></figure>

Debemos tener en cuenta que la VM viene con dos adaptadores de red:

* adaptador de red 1: Host-only
* adaptador de red 2: NAT

Una vez la VM está funcionando lo que vamos a ver es algo como la siguiente imagen: ![image](https://github.com/user-attachments/assets/26c9d2aa-8496-4612-9f45-d3157b8b2ef1)

## Testeando

<figure><img src="../.gitbook/assets/image (446).png" alt=""><figcaption></figcaption></figure>

## Imágenes

Falta ...

*

## Links

* Cisco - imágenes de dispositivos
  * [https://ccnadesdecero.es/descargar-cisco-ios-gns3/](https://ccnadesdecero.es/descargar-cisco-ios-gns3/)
  * [https://drive.google.com/drive/folders/102jxZ9ECpe6ZFtXYdK\_81iEVuuFoGOGR](https://drive.google.com/drive/folders/102jxZ9ECpe6ZFtXYdK_81iEVuuFoGOGR)
  * [https://gns3.com/marketplace/appliances](https://gns3.com/marketplace/appliances)
  * [https://wiki.opnsense.org/manual/install.html](https://wiki.opnsense.org/manual/install.html)
  * [https://www.youtube.com/watch?v=47HWtHtbjxY](https://www.youtube.com/watch?v=47HWtHtbjxY)
  * [https://www.youtube.com/watch?v=yqhrAs58y5c\&t=85s](https://www.youtube.com/watch?v=yqhrAs58y5c\&t=85s) - qemu
  * https://www.youtube.com/watch?v=Ibe3hgP8gCA\&list=PLhfrWIlLOoKNFP\_e5xcx5e2GDJIgk3ep6 <mark style="color:purple;">**\***</mark>
  * [https://www.youtube.com/watch?v=2aI6ipuJPLo](https://www.youtube.com/watch?v=2aI6ipuJPLo) \*
  * [https://www.youtube.com/watch?v=tBswpi22q\_0](https://www.youtube.com/watch?v=tBswpi22q_0) \*\*
  * [https://www.youtube.com/watch?v=W2benl0R-A4](https://www.youtube.com/watch?v=W2benl0R-A4) \*\*
* Instalación en Linux y específicamente en Debian 12
  * [https://www.javiercd.es/posts/redes/instalar\_gns3\_debian12/instalar\_gns3\_debian12](https://www.javiercd.es/posts/redes/instalar_gns3_debian12/instalar_gns3_debian12)
  * [https://docs.gns3.com/docs/getting-started/installation/linux](https://docs.gns3.com/docs/getting-started/installation/linux)
* Cursos - training
  * [https://gns3.teachable.com/l/products?sortKey=name\&sortDirection=asc\&page=1](https://gns3.teachable.com/l/products?sortKey=name\&sortDirection=asc\&page=1)
