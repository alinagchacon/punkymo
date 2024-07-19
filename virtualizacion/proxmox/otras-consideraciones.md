---
description: Proxmox
---

# Otras consideraciones

Proxmox se basa en tecnologías de virtualización asistida por hardware como KVM para permitir la creación y ejecución eficiente de VM en servidores físicos, proporcionando mejor rendimiento y seguridad en entornos de virtualización.&#x20;

Se aprovechan al máximo las características de virtualización de hardware de los procesadores modernos para crear y ejecutar máquinas virtuales de un modo más eficiente y con mayor rendimiento.  Algunas de las ventajas de esto son:

**Rendimiento**: Las VM aprovechan el poder de procesamiento de la CPU y accedeny consiguen un mejor rendimiento.

**Seguridad**: Mejora la seguridad  y reduce las posibilidades de ataques entre máquinas virtuales.

**Aislamiento**: Proporciona alto grado de aislamiento entre las VM, lo que significa que una VM no puede acceder ni afectar directamente a otras en el mismo host.

A grandes rasgos se trata de verificar si lo tenemos habilitado o no. Para ello nos vamos al Administrador de tareas, a la pestaña de la CPU y buscamos el estado de la virtualización del equipo.

Si la virtualización está habilitada con Hyper-V tendremos que deshabilitar el Hyper-V si queremos virtualizar sistemas de 64bits o no podríamos crear VM de 64bits con VirtualVBox.

Para deshabilitar Hyper-V nos vamos a Activar a desactivar características de Windows, buscamos Hyper-V y lo desmarcamos la opción habilitada.&#x20;

Si lo tenemos habilitado entonces, podemos entrar a la configuración de la VM creada y buscar el apartado de Sistema - Aceleración y podemos ver el tipo de virtualización habilitada en el desplegable de paravirtualización.

Algunas de las siguientes opciones se encuentran entre las características a seleccionar:

**Ninguno**: VirtualBox no aprovecha ningún tipo de tecnología de virtualización.

**Predeterminado**: habilitado automáticamente al crear la VM. Utiliza el método más adecuado en función del S.O. que hayamos seleccionado. La recomendable por defecto.

**KVM**: interfaz de hipervisor para Linux a partir de la versión 2.6.25 del Linux Kernel. Cualquier S.O. que haga uso de esa versión o cualquier versión posterior del Linux Kernel, deberá utilizar este modo de paravirtualización. Iigualmente, se selecciona automáticamente con la configuración por defecto.

<figure><img src="../../.gitbook/assets/image (237).png" alt=""><figcaption><p>Pestaña de virtualización en una VM d eVirtualBox</p></figcaption></figure>

En VMware tenemos:

<figure><img src="../../.gitbook/assets/image (238).png" alt=""><figcaption><p>Virtualización en VMware</p></figcaption></figure>

Digamos que es fundamental tener bien configurado nuestro VirtualBox o el VMware player si queremos que nuestro hipervisor de Proxmox funcione correctamente. De hecho,  de no corregirlo nos mostraría un fallo como el siguiente al instalar Proxmox:

<figure><img src="../../.gitbook/assets/image (239).png" alt=""><figcaption><p>Mensaje de error en el proceso de instalación de Proxmox</p></figcaption></figure>



### Comandos para gestionar VirtualBox

<mark style="color:red;">falta</mark>

