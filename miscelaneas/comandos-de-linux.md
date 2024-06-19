---
description: Mi libreta de apuntes
---

# Comandos de Linux

Este es un sitio muy interesante para aprender linux, cifrado entre otros: [<mark style="color:blue;">`https://overthewire.org/wargames/`</mark>](https://overthewire.org/wargames/)

### gráfica y drivers - lspci - lsmod&#x20;

```
lsmod | grep "kms\|drm"

sudo lspci -k | grep -EA3 'VGA|3D|Display'

sudo lshw -c video
```

### Específico de Ubuntu

Un comando específico de Ubuntu que proporciona información de las distribuciones de Ubuntu

```
sudo ubuntu-distro-info 
```

Un comando específico de Ubuntu para verificar los drivers&#x20;

```
sudo ubuntu-drivers
```

### dmesg

Muestra los mensajes del kernel, incluyendo aquellos relacionados con la tarjeta gráfica y sus drivers.&#x20;

```sh
dmesg | grep -i 'drm\|nvidia\|nouveau\|radeon'

```

#### `inxi`

`inxi` es una herramienta de línea de comandos que proporciona información del sistema.&#x20;

```sh
inxi -G
```

### glxinfo

Brinda información sobre la configuración gráfica y los drivers OpenGL

```
glxinfo | grep 'OpenGL'
```

### lsb\_release

Muestra información específica de la distribución

```
lsb_release -a
```

### uname -a&#x20;

Muestra información del sistema

```
uname -a
```

### cat

Mostrar contenido de archivos de texto plano

Si el archivo se nombra como: -&#x20;

<pre><code><strong>cat &#x3C; -
</strong></code></pre>

### more

El comando _more_ tiene una doble funcionalidad, _muestra_ el contenido del fichero por páginas:

```
more file.txt 
```

Si está combinado con otro comando como _ls_, te muestra por páginas el contenido del directorio:

```
 ls -l /etc | more
```

### find

Permite realizar búsquedas de ficheros en directorios&#x20;

```
find / -type f -size 33c -user john -group admin
grep -i bandit file.txt 
grep  patrón file.txt

```

### _snap_

_Sistema de gestión de paquetes desarrollada por Canonical. Introducido en la versión 16.04. Se trata de una nueva forma de instalar aplicaciones en Ubuntu._

_El siguiente comando te permite ver la lista de paquetes que tienes en el sistema y que utiliza snap._

```
snap list
```

_Si quieres ver / parar los servicios de snap, entonces tendrías que ir a:_

```
sudo systemctl status snapd.service 
sudo systemctl status snapd.socket
sudo systemctl status snapd.seeded.service 
```

### _s_ort&#x20;

### uniq&#x20;

### strings&#x20;

### base64

### tr&#x20;

### tar, gzip, bzip2&#x20;

### xxd

### sudo so-status&#x20;

### LINKS

* https://juncotic.com/grep-y-las-expresiones-regulares-basicas-y-extendidas/ &#x20;
* https://javiermartinalonso.github.io/linux/2018/01/15/linux-grep-patrones-debug.html&#x20;

## &#x20;
