# QEMU

### Introducción <a href="#requisitos-previos" id="requisitos-previos"></a>

#### Características del sistema <a href="#requisitos-previos" id="requisitos-previos"></a>

Antes de comenzar nos tenemos que asegurar que nuestro sistema cumple con los requisitos necesario. Esto es:

* **Procesador -** con soporte de virtualización (Intel VT-x o AMD-V). Debemos comprobar que en la BIOS/UEFI está habilitada la virtualización.
* **Debian** - yo lo estoy probando en un Debian 12.

#### Actualizar el sistema <a href="#paso-1-actualizar-el-sistema" id="paso-1-actualizar-el-sistema"></a>

Siempre es recomendable actualizar el sistema. Para ello, abrimos un terminal y ejecutamos:

<pre><code><strong>sudo apt update &#x26;&#x26; apt upgrade -y
</strong></code></pre>

#### Soporte de virtualización <a href="#paso-2-comprueba-de-soporte-de-virtualizacion" id="paso-2-comprueba-de-soporte-de-virtualizacion"></a>

Ahora nos toca verificar si la CPU soporta la virtualización. Para ello, ejecutamos el siguiente comando:

```
sudo egrep -c '(vmx|svm)' /proc/cpuinfo
```

Si el resultado es 0, la CPU no soporta virtualización o al menos no está habilitada en la BIOS/UEFI. Si el resultado es 1 o más, quiere decir que la CPU soporta virtualización.

#### CPU Checker <a href="#paso-3-instalar-el-cpu-checker" id="paso-3-instalar-el-cpu-checker"></a>

Instalaremos la herramienta <mark style="color:purple;">cpu-checker</mark>  que no permite verificar si se puede utilizar KVM en nuestro sistema, como root:

<pre><code><strong>sudo apt install cpu-checker -y
</strong></code></pre>

Una vez instalada la herramienta, podemos ejecutar el siguiente comando para comprobar si KVM es soportado como root:

```
kvm-ok
```

Se debe mostrar un mensaje que indica que KVM está habilitado.

```
INFO: /dev/kvm exists
KVM acceleration can be used
```

#### Instalar KVM, Qemu y Virtmanager <a href="#paso-4-instalar-kvm-qemu-y-virtmanager" id="paso-4-instalar-kvm-qemu-y-virtmanager"></a>

Ahora si nos disponemos a instalar KVM, Qemu y Virtmanager como usuario root:

```
apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager -y
```

#### Verificar la instalación <a href="#paso-5-verificar-la-instalacion" id="paso-5-verificar-la-instalacion"></a>

Una vez instalados los paquetes, verificamos que el servicio de <mark style="color:purple;">libvirt</mark> esté activo como root:

```
sudo systemctl status libvirtd
```

#### Añadir el usuario al grupo libvirt <a href="#paso-6-anadir-tu-usuario-al-grupo-libvirt" id="paso-6-anadir-tu-usuario-al-grupo-libvirt"></a>

Ahora, podemos añadir nuestro usuario al grupo <mark style="color:purple;">libvirt</mark> y al grupo <mark style="color:purple;">kvm</mark> para poder gestionar las VM sin necesidad de usar <mark style="color:purple;">sudo</mark>:

```
sudo usermod -aG libvirt kirby
sudo usermod -aG kvm kirby
```

Para que los cambios surtan efecto, cerramos la sesión y volvemos  a iniciarla. Mejor si reiniciamos el sistema.



<br>
