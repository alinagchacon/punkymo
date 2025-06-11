---
description: Cisco
---

# CML

## ¿Qué es CML?

Cisco Modeling Labs es una plataforma de simulación de redes que permite diseñar y probar redes virtuales que se ejecutan en estaciones de trabajo y servidores. No solo permite simular redes Cisco sino de también de otros fabricantes utilizando imágenes reales  que nos permiten crear simulaciones de red muy fiables. Un laboratorio virtual que permite la simulación de redes en un entorno controlado.&#x20;

En resumen:

* Es una potente herramienta de simulación de redes que utiliza imágenes reales de dispositivos Cisco.
* Utiliza una interfaz HTML5 y una API.
* Facilita el acceso práctico a Cisco IOS y a imágenes de software virtualizadas para experiencias de laboratorio realistas.
* Es ideal para estudiantes, educadores, y cualquiera que se esté preparando para los exámenes de certificaciones Cisco, especialmente para CCNA o CCNP.
* Ayuda a alumnos e instructores a crear, testear, solucionar problemas de topologías de red en un entorno virtual controlado.

Algunas de las características de CML son:

* **Simulación de Red Virtualizada** -permite la creación de redes complejas utilizando dispositivos Cisco virtualizados, facilitando la experimentación con configuraciones de red sin necesidad de hardware físico.
* **Experiencia Real con CLI de Cisco -** brinda acceso a la CLI real de Cisco. Soporta la emulación de hardware/software de terceros mediante imágenes virtuales `QCOW2` para `KVM`.
* **Protocolos y Características** - Soporta protocolos avanzados como BGP, OSPF, EIGRP, MPLS y VXLAN. Incluye capacidades de enrutamiento y conmutación de Capa 2 y Capa 3.



Si hacemos una comparativa con Cisco Packet Tracer tenemos que:

<table><thead><tr><th width="192.54541015625">Cisco</th><th width="201.727294921875">CML</th><th>Observaciones</th></tr></thead><tbody><tr><td>Simulador</td><td>Emulador</td><td></td></tr><tr><td>CPU básico, 2GB de RAM.</td><td>CPU con 4 núcleos, 8GB de RAM y VTx/EPT.</td><td>Packet Tracer se instala fácilmente, mientras que CML requiere de una cuenta y unos requerimientos de HW.</td></tr><tr><td>Simulas decenas de dispositivos.</td><td>Simulas un máx. de 5 nodos simultáneos.</td><td>En la versión CML - free solo puedes simular 5 nodos.</td></tr><tr><td>Comandos y SO limitado a CCNA</td><td>Comandos y SO con imágenes reales y Linux</td><td>El problema son las imágenes reales</td></tr><tr><td>No dispone de API</td><td>Dispone de una API para conectar con redes reales.</td><td></td></tr></tbody></table>

Por tanto, Cisco CML free emula dispositivos reales y utiliza imágenes virtuales reales de Cisco IOS proporcionando una experiencia real del mundo, mientras que Cisco Packet Tracer simula el comportamiento de la red e incluye una potente visualización a nivel de paquetes, más eficaz para aprender y solucionar problemas de conceptos de redes de forma interactiva.

## Requerimientos, cuenta  y descarga

La versión CML - free es completamente gratis y no necesita de licencia. Sin embargo, requiere una cuenta de Cisco.com completamente cumplimentada. \


### Requisitos del sistema

Como decíamos anteriormente, para instalar CML  en una VM necesitamos de:&#x20;

* CPU de 4 núcleos, 8 GB de RAM.
* Soporte VT-x o AMD-V para capacidades de virtualización anidada
* Instalación en bare metal o en máquina virtual (VMWare, Nutanix, Proxmox, Hyper-V, etc.)
* La versión free permite un total de 5 dispositivos o nodos conectados simultáneamente.
* Está limitado a ASAv, IOLv, IOLvL2, nodos host: Ubuntu Linux, Alpine Linux y una imagen de escritorio
* Los conectores externos y los switches no gestionados no cuentan para el límite de 5 nodos.

## Tipos de Instalación

Se dispone de varias opciones de instalación que son:

1\) **Entorno virtual**:&#x20;

* En VM compatible con Hyper-V, VMware, Proxmox, etc.&#x20;
* Requiere virtualización anidada

2\) **Bare Metal**:

* Instalación directa en un servidor físico, dedicado exclusivamente a CML.
* Tiene mejor rendimiento frente a las VM, utilizando el mismo hardware.

3\) **Nube (AWS / Azure) (BETA)**:

* Puede desplegarse en la nube usando una toolchain especializada.
* Ideal para laboratorios remotos o escalables.
* Requiere revisión detallada de requisitos y documentación específica para cada plataforma.



## Instalando&#x20;

Como he mencionado anteriormente, es necesario disponer de una cuenta en Cisco debidamente cumplimentada. Es libre, pero no deja de ser un proceso engorroso que voy a obviar porque proporcionaré los dos archivos que necesitamos para hacer la instalación:

* La OVA de la VM para instalar en VMware
* La ISO de la plataforma de referencia (refplat) que contiene las imágenes de los dispositivos de red (routers, switches) a utilizar en los laboratorios. Este archivo se carga en CML después de la instalación inicial.

📝 Note:  Si quieres información oficial sobre cómo hacer la descarga puedes dirigirte a: [https://software.cisco.com](https://software.cisco.com). Si quieres información oficial sobre cómo realizar la instalación puedes clicar en: [https://developer.cisco.com/docs/modeling-labs/installing-cml-as-vm/](https://developer.cisco.com/docs/modeling-labs/installing-cml-as-vm/).

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>OVA y REFPLAT </p></figcaption></figure>

En resumen, para hacer una instalación limpia del CML necesitamos de:

* cml2\_f\_2.8.1-14\_amd64-35.ova
* refplat-20241016-freetier-iso.zip
* VMware Workstation instalado.



### Pasos de instalación:

* Debemos importar la OVA de la VM. Para ello podemos hacer click con el botón secundario encima del archivo y abrir con VMware Workstation para comenzar a importar.
* Una vez hecho esto, debemos ver que nuestra VM tiene como características de HW: 8GB de RAM, 4 procesadores, un hdd scsi de 32GB y viene por defecto en adaptador puente.&#x20;

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt="" width="343"><figcaption><p>Características de HW de la VM de CML</p></figcaption></figure>

Editaremos la VM de modo que cambiamos el adaptador de red a NAT y nos disponemos a instalar. -&#x20;

* Durante el proceso nos pedirá aceptar el EULA para continuar, algo que deberíamos leer y no hacemos habitualmente.&#x20;
* Dejaremos las siguientes opciones por defecto hasta que nos pida añadir la reference plattform image en el CD/DVD.
  * Clicamos en VM > Settings, conectamos el dispositivo y añadimos la ISO de la plataforma de referencia una vez descomprimo el archivo.
  * Guardamos y salimos para continuar el proceso de instalación.

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>Añadiendo la refplat a la instalación de la VM </p></figcaption></figure>



* El siguiente punto donde nos detenemos es en la contraseña de los usuarios sysadmin y admin. En mi caso, también he mantenido los mismos por defecto.
* Dejamos que la IP de la VM sea por DHCP y comienza la última etapa de la instalación.

Una vez finalizado tomemos nota de la IP (resaltada en la imagen siguiente) para acceder a nuestra plataforma. Esto es: **https://192.168.116.129**.

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt="" width="563"><figcaption><p>La IP que debemos utilizar para acceder a la plataforma instalada desde el navegador</p></figcaption></figure>

Accediendo desde el navegador sería:

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt="" width="375"><figcaption><p>Accediendo desde el navegador</p></figcaption></figure>

En el siguiente artículo explico cómo trabajar con CML.

## Links

* [https://learningnetwork.cisco.com/s/question/0D56e0000EBtbrbCQB/get-familiarized-with-cisco-modeling-labs-explore-ccna-topologies-created-by-your-peers?mkt\_tok=NTY0LVdIVi0zMjMAAAGa0SZMJwmzcS42QpMw5ABvHj-TkNMsizosNc6UIdjLg7iqOgaN0sk2QpwJxowSlwi-rakRCsUbztf0td1BAvUq--dF4l74ZYqfH2JeFCZK5X9ouR4I](https://learningnetwork.cisco.com/s/question/0D56e0000EBtbrbCQB/get-familiarized-with-cisco-modeling-labs-explore-ccna-topologies-created-by-your-peers?mkt_tok=NTY0LVdIVi0zMjMAAAGa0SZMJwmzcS42QpMw5ABvHj-TkNMsizosNc6UIdjLg7iqOgaN0sk2QpwJxowSlwi-rakRCsUbztf0td1BAvUq--dF4l74ZYqfH2JeFCZK5X9ouR4I)&#x20;
* [https://github.com/CiscoDevNet/cml-community/tree/master/node-definitions/cisco](https://github.com/CiscoDevNet/cml-community/tree/master/node-definitions/cisco)&#x20;
* [https://developer.cisco.com/docs/modeling-labs/reference-platforms-and-images/](https://developer.cisco.com/docs/modeling-labs/reference-platforms-and-images/)
* [https://developer.cisco.com/docs/modeling-labs/installing-cml-as-vm/](https://developer.cisco.com/docs/modeling-labs/installing-cml-as-vm/)
* [https://developer.cisco.com/docs/modeling-labs/initial-setup/](https://developer.cisco.com/docs/modeling-labs/initial-setup/)
* [https://software.cisco.com/download/home/286193282/type/286326381/release/CML-Free](https://software.cisco.com/download/home/286193282/type/286326381/release/CML-Free)
* [https://www.youtube.com/watch?v=og9320luq20](https://www.youtube.com/watch?v=og9320luq20)



[![GitBook](https://img.shields.io/static/v1?message=Documented%20on%20GitBook\&logo=gitbook\&logoColor=ffffff\&label=%20\&labelColor=5c5c5c\&color=3F89A1)](https://www.gitbook.com/preview?utm_source=gitbook_readme_badge\&utm_medium=organic\&utm_campaign=preview_documentation\&utm_content=link)

