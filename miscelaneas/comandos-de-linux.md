---
description: Mi libreta de apuntes
---

# Comandos de Linux

Este es un sitio muy interesante para aprender linux, cifrado entre otros: [<mark style="color:blue;">`https://overthewire.org/wargames/`</mark>](https://overthewire.org/wargames/)



### gráfica y drivers - lspci - lsmod&#x20;

```
lsmod | grep "kms\|drm"

sudo lscpi -k | grep -EA3 'VGA|3D|Display'

sudo lshw -c video
```

### lsb\_release -a



### uname -a&#x20;



### cat

Mostrar contenido de archivos de texto plano

Si el archivo se nombra como: -&#x20;

_cat < -_

### more

El comando _more_ tiene una doble funcionalidad, _muestra_ el contenido del fichero por páginas:

_more file.txt_&#x20;

Si está combinado con otro comando como _ls_, te muestra por páginas el contenido del directorio:

&#x20;_ls -l /etc | more_&#x20;

### find

_find / -type f -size 33c -user john -group admin_

_grep -i bandit file.txt_&#x20;

_grep  patrón file.txt_





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
