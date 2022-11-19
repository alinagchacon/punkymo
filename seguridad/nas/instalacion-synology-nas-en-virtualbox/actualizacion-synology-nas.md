---
description: JLM
---

# Actualización Synology NAS

Para actualizar el NAS a través de Tinycore es diferente de si se tratara de un NAS original. Cuando instalamos el NAS a través de SSH lo hicimos para una versión especifica, por lo tanto al actualizarlo desde el NAS normal, al reiniciarse no encuentra la versión esperada.

Por lo tanto para conseguir actualizar el NAS debemos hacer una nueva instalación desde Tinycore, además de otros comandos que explicare a continuación.

> Antes de empezar la actualización recomiendo hacer una instantánea o una copia de seguridad de los datos almacenados, ya que equivocarnos en un paso o producirse un error durante la actualización puede suponer no volver a acceder al NAS.

### Primer paso - Actualizar DSM desde la propia interfaz

Antes de entrar otra vez al terminal de Tinycore debemos realizar la actualización de manera normal. Para ello accedemos al **Panel de control**, este lo encontramos en el escritorio o en las aplicaciones. Después de eso accedemos al apartado **Actualizar y restaurar**.&#x20;

Desde ahí descargamos la actualización y la instalaremos normalmente, debemos fijarnos que versión esta descargándose para posteriormente seleccionarla en Tinycore.

Como podemos ver en la siguiente imagen la versión que se ha descargado es la **7.1.1-42962**, por lo que deberemos apuntarla para posteriormente recordarla.

<figure><img src="../../../.gitbook/assets/image (10) (1).png" alt=""><figcaption><p>Actualización desde el Panel de control</p></figcaption></figure>

Ahora que ya sabemos la versión podemos darle a **Actualizar ahora**. Debemos aceptar que el NAS se reiniciará y comenzará la actualización. Una vez que ya la tengamos actualizada, el NAS se reiniciara. Antes de que se vuelva a encender, desde el GRUB debemos seleccionar la opción de **Tinycore Image Build.**

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Selección Tinycore en GRUB</p></figcaption></figure>

### Segundo paso - Acceder a Tinycore

Para acceder al terminal de Tinycore podemos volver a conectarnos a través de SSH para una manipulación mas cómoda.

Usuario: **tc**

Contraseña: **P@ssw0rd**

```
ssh tc@x.x.x.x
```

<figure><img src="../../../.gitbook/assets/image (222).png" alt=""><figcaption><p>Conexión SSH</p></figcaption></figure>

### Tercer Paso - Comprobar actualizaciones

Muchos de los pasos que haremos son parecidos a la propia instalación aunque hay algunos que son nuevos.

Para comprobar las actualizaciones primero ejecutaremos el siguiente comando, en el caso que hubiese las aceptaremos:

```
sudo ./rploader.sh update
```

Después actualizaremos todos los archivos a través del siguiente comando, aunque no aparezca ninguna actualización:

```
sudo ./rploader.sh fullupgrade
```

### Cuarto paso - Generar una nueva MAC

Al tratarse de una "nueva instalación" debemos volver a generar una nueva MAC. Para ello debemos ejecutar el siguiente comando:

```
sudo ./rploader.sh serialgen DS3615xs
```

<figure><img src="../../../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

Cabe recordar que esta nueva dirección MAC debemos apuntarla en algún lugar ya que posteriormente la necesitaremos modificar en VirtualBox.

En este caso la nueva MAC es la siguiente: **00:11:32:E2:1F:E1**

### Quinto paso - Crear nueva instalación

Ahora debemos realizar la nueva instalación, introduciendo el nombre de la nueva versión. Si no recordamos como se escribe podemos verlo introduciendo el siguiente comando:

```
sudo ./rploader.sh
```

En este caso el nombre de la nueva versión es **ds3615xs-7.1.1-42962**, puede variar dependiendo de las nuevas versiones de DSM. Por lo tanto en este caso el comando a ejecutar es el siguiente:

```
sudo ./rploader.sh build ds3615xs-7.1.1-42962
```

Después de que finalice la instalación debemos ejecutar otro comando llamado **postupdate**. Este sirve para que el sistema entienda de que esto se trata de una actualización.

Para ello ejecutamos el siguiente comando:

```
sudo ./rploader.sh postupdate ds3615xs-7.1.1-42962
```

Al ejecutar este comando nos solicitara varias veces si queremos instalar las nuevas updates, debemos aceptar todas, ya que al descargar el archivo desde el NAS también incluye las nuevas updates.

### Sexto paso - Realizar backups

En este paso debemos realizar los backups de todo lo que hemos hecho para que a la hora de reiniciar el sistema se guarden todos los cambios

Para ello ejecutamos el siguiente comando:

```
sudo ./rploader.sh backup
```

También debemos hacer backup del cargador de arranque, que se hace con el siguiente comando:

```
sudo ./rploader.sh backuploader
```

Por último, debemos apagar Tinycore para cambiar la MAC en VirtualBox, sin este paso no funcionara la actualización:

```
exitcheck.sh poweroff
```

### Séptimo paso - Cambiar MAC en VirtualBox

Para cambiar la MAC debemos acceder a la configuración de red de la máquina virtual y sustituir la anterior MAC por la nueva.

En este caso debemos cambiar esta MAC: **00113294688A** por esta nueva: **001132E21FE1**

### **Octavo paso - Arrancar el NAS**

Una vez ya modificada la MAC podemos encender el NAS, debemos estar atentos al GRUB ya que debemos de seleccionar de nueva la opción de **(SATA boot)**.

Si todo a funcionado correctamente deberíamos volver a acceder al NAS con el mismo usuario y contraseña. Para comprobar que realmente se han instalado las actualizaciones volvemos acceder al Panel de control, al apartado de **Actualizar y restaurar**. En el podemos ver que si está instalada la última versión.

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption><p>DSM actualizado a la última versión</p></figcaption></figure>

