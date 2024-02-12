---
description: Un comando para backup
---

# Rsync

Se trata de una herramienta por línea de comandos para sistemas basados en Unix y Linux que permite la sincronización y copia de archivos y directorios entre sistemas locales y remotos. Las características principales son:

1. Está diseñado para copiar solo las diferencias entre los archivos, lo que hace que la transferencia de datos sea más eficiente dado que ahorra ancho de banda y tiempo al actualizar archivos y directorios. Ideal para realizar copias de seguridad incrementales, ya que solo copia los archivos que han cambiado desde la última sincronización.
2. Te permite especificar patrones de archivos o directorios que deben ser excluidos de la sincronización.
3. Puede comprimir datos durante la transferencia y utilizar conexiones SSH para asegurar la transmisión de datos a través de la red.
4. Permite copiar archivos y directorios localmente en un mismo equipo y para copias entre sistemas remotos a través de SSH u otros protocolos.
5. Por defecto, preserva los atributos y permisos, marcas de tiempo, propietarios y grupos de archivos durante la copia.
6. Puede reanudar una transferencia interrumpida sin necesidad de volver a copiar todo desde cero.

La sintáxis de `rsync` para copiar un directorio local a un servidor remoto utilizando SSH es:

```bash
rsync [opciones] carpeta_origen carpeta_destino
rsync -avz /ruta/local usuario@servidor:/ruta/remota
```

donde:

* `/ruta/local` es la ubicación local que deseas copiar.
* `usuario` es el nombre de usuario en el servidor remoto.
* `servidor` es la dirección del servidor remoto.
* `/ruta/remota` es la ubicación en el servidor remoto donde deseas copiar los archivos.

## Pruebas&#x20;

En distribuciones como Ubuntu viene instalado por defecto pero en todo caso puedes comprobarlo:

```
whereis rsync
```

Y si no lo tuvieras, pues toca instalar:

```
sudo apt install rsync
```

### Crear directorios de prueba: origen y destino

Vamos a realizar algunas pruebas de uso y para ello, lo primero que vamos a hacer será crear un directorio con ficheros de contenido que nos sirvan para testear en local.&#x20;

En caso de no disponer de archivos que podamos utilizar para nuestras pruebas podemos crearlos con el comando `touch`.

```
mkdir carpeta_origen
touch file{1..20}
```

Creamos otra carpeta de destino a donde irían los archivos copiados:

```
mkdir carpeta_destino
```

Por ejemplo, en la imagen siguiente tengo el contenido de la carpeta de origen donde ya tenía un archivo file1.txt:&#x20;

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption><p>Carpeta de origen</p></figcaption></figure>

### Opciones de rsync

Veamos algunas de las opciones más importantes de `rsync`. De hecho, para ver todas las opciones puedes usar el comando:

```
rsync --help
```

* \--verbose, -v - muestra todo el proceso
* \--archive, -a  - archive mode is -rlptgoD (no -A,-X,-U,-N,-H)
* \--recursive, -r recurse into directories
* \--update, -u skip files that are newer on the receiver
* \--links, -l copy symlinks as symlinks
* \--acls, -A preserve ACLs (implies --perms)&#x20;
* \--xattrs, -X preserve extended attributes

No son los únicos pero los más utilizados si. Veamos ejemplos de uso y verifica las diferencias entre las diferentes opciones.

### Probando de un directorio a otro dentro del sistema

Si quiero copiar de manera recursiva el contenido de un directorio en otro:

```
rsync -r origen/ destino/
```

Si quiero ver el proceso de la copia y que se mantengan los permisos y atributos de los archivos:

```
rsync -rva origen/ destino/
```

_Nota: cuidado con poner o no "/" al final del nombre de las carpetas_.

El tipo de copia que se ha realizado es una copia completa. No fuera demasiado eficiente si siempre fuera así. Por suerte, rsync hace copias incrementales. Para comprobarlo, modifiquemos un archivo de la carpeta de origen. Yo modifiqué el archivo `file1` y volví a ejecutar `rsync. C`omo se muestra en la imagen el único archivo que se ha copiado ha sido precisamente el que hemos modificado: `file1`.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>copia incremental con rsync</p></figcaption></figure>

### Recuperando el contenido

Supongamos que hubiéramos perdido el contenido de la carpeta origen. Para hacer más realista la prueba vamos a eliminar todo el contenido de la carpeta origen, incluso podemos probar a eliminar toda la carpeta origen.

```
rm -r origen/*
```

Entonces, para recuperar toda la información podemos intercambiar la carpeta de origen por destino y recuperar los datos:

```
rsync -rav destino/ origen/
```

Este mismo proceso se puede realizar con un `pendrive` o `hdd` externo que tengamos conectado a nuestro equipo. Solo tienes que tener claro el path hasta el directorio de interés.



### Copiando de un directorio a un pendrive

Algo que perfectamente pudiéramos querer hacer es copiar a un pendrive. Como estoy haciendo las pruebas en un Ubuntu Server, al insertar mi pendrive en el USB no se "monta" automáticamente con lo cual tuve que hacerlo manual. Para ello hacemos lo siguiente:

Crear un directorio dentro /media: Para ello usamos el comando:\


```
mkdir /media/usb
```

Ahora identificamos el nombre de la unidad que queremos montar. Para ello usamos el comando:

```
ls -l /dev/sd*
```

Nos debe mostrar el listado de /dev/sda, /dev/sda1,  /dev/sda2 que tengamos. En mi caso, solo tengo un disco duro, sin particionar, por tanto,  /dev/sda1.

Una vez que tengamos conectados el USB y volvemos a hacer ls -l:

```
ls -l /dev/sd*
```

Veremos que aparece en el listado /dev/sdb1 que es nuestro pendrive.

Montamos la memoria USB con el comando:

```
mount -t exfat /dev/sdb1 /media/usb
```

Para realizar cualquier operación con nuestro pendrive, tenemos que ir a la localización `/media/usb`.

Cuando hayamos terminado, debemos desmontar la memoria y para ello:

```
umount /media/usb
```

### Copiando archivos entre equipos remotos

Veamos la parte más interesante del comando que es precisamente la copia de archivos entre sistemas remotos. Supongamos ahora que la carpeta de destino está en un equipo remoto. Para hacer este tipo de pruebas necesitamos alguna de las siguientes opciones:

1. dos VM conectadas en modo redNAT
2. una VM conectada en modo adaptador puente con la red en la que estás, para utilizar tu propio equipo host
3. una VM conectada en modo red only host que conecta directamente con tu equipo host.

Yo dispongo de un equipo que tiene Ubuntu instalado y una VM con Ubuntu en modo adaptador puente. Los datos son como sigue:

| Server - VM Ubuntu | Cliente - Equipo host |
| ------------------ | --------------------- |
| 192.168.1.78       | 192.168.1.15          |
| carpeta "backups"  | carpeta "origen"      |

Vamos a tomar el mismo contenido que tenemos creado en la carpeta origen y lo vamos a pasar al servidor  utilizando rsync.

```
rsync -rav origen/ punky@192.168.1.78:backups/
```

donde:

* punky: es el usuario del servidor en la MV
* backups: es la carpeta destino de la copia de seguridad

Habrás podido observar que te pide el password del usuario del equipo server que recibirá la copia de la información.

Un detalle adicional es que necesitamos tener instalado el servicio SSH en el servidor, que en nuestro caso sería la MV, aunque yo lo tengo instalado en ambos equipos.

### Automatizando el proceso de copia

Lo visto anteriormente sigue sin ser lo suficientemente eficiente como para poder utilizar rsync para crear copias de seguridad automáticamente. Para ello, vamos a instalar una herramienta que no me acaba de gustar del todo por la falta de seguridad que implica. Se trata de `sshpass`.  Para instalarlo, basta con hacer:

```
sudo apt install sshpass
```

**SSHPASS** - Es una utilidad de línea de comandos nos permite proporcionar una contraseña a un programa SSH en lugar de ingresarla manualmente cuando se establece la conexión SSH, que es justo lo que nos ha sucedido.&#x20;

Esto puede ser útil en situaciones en las que necesitas automatizar tareas que implican conexiones SSH y no puedes utilizar métodos de autenticación más seguros, como el uso de claves SSH o certificados.

El uso de `sshpass` puede ser conveniente, pero plantea problemas de seguridad, puesto que implica almacenar contraseñas en texto plano en scripts o archivos de configuración. Esto hace que sea menos seguro que el uso de claves SSH o certificados, que son métodos de autenticación más seguros.

Para usar `sshpass`, debes proporcionar la contraseña como argumento o a través de un archivo de texto, y éste se encargará de entregarla al comando SSH que deseamos ejecutar.&#x20;

### Crear un script

Vamos a crear un script que realice la copia de seguridad. Para ello vamos a crear un archivo llamado `rsync.sh` y lo vamos a editar con nano, en mi caso:

```
#!/bin/bash
# sincroniza carpeta desde servidor hacia equipo cliente
sshpass -p 'mi_password' rsync --progress -avz -e ssh kirby@192.168.1.15:~/Documentos/origen ~/backups/
```

Para ejecutar el script debes escribir:

```
bash rsync.sh
```

Y como podrás observar, realizará la copia de seguridad sin tener que teclear la contraseña porque ya la tiene en el script.

Por último, vamos a programar una tarea que ejecute el script cada cierto tiempo. Para ello,  vamos a ver el `crontab`.

### Cron

Para crear una tarea programada en `cron`, debemos utilizar el comando `crontab` que nos permite editar o crear un archivo `cron` con las instrucciones de la tarea que deseamos automatizar.  Para editar el archivo `cron`, ejecutamos el  comando:

```bash
crontab -e
```

La primera vez que creas una tarea con `crontab`, el sistema te preguntará el tipo de editor de texto a utilizar. Aquí es donde yo selecciono nano aunque puedes elegir otro, por ejemplo, `vim`, `emacs`.

En el `cron` cada línea representa una tarea programada y mantiene un formato específico. La estructura general es la siguiente:

```plaintext
* * * * * comando_a_ejecutar
```

Donde los cinco asteriscos representan la programación de tiempo. Cada asterisco hace referencia a los minutos, horas, días del mes, meses, días de la semana.

La siguiente imagen nos lo muestra mejor:

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption><p>crontab</p></figcaption></figure>

Si queremos  ejecutar nuestro script llamado `rsync.sh` todos los días a las 22:00h, la línea en el archivo `cron` sería:

```plaintext
00 22 * * * ~/scripts/rsync.sh
```

Aquí, 00 representa los minutos, 22 las horas, y los asteriscos significan todos los días del mes, todos los meses y todos los días de la semana. La tarea programada estará en funcionamiento según la programación establecida y para verificarlo ejecutamos el siguiente comando:

```bash
crontab -l
```

Que nos mostrará la lista de tareas programadas del archivo `cron.`

Prueba y verifica la copia!

