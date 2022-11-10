---
description: Instalación de DSM sobre VirtualBox
---

# Synology NAS (DSM)

DSM es el sistema operativo que usan los NAS de la marca Synology. Este sistema es muy intuitivo y fácil de usar, además es muy útil a la hora de conectarse con diferentes usuarios. En este caso DSM lo instalaremos sobre otro sistema operativo, TinyCore de Redpill. Es una distribución de RedHat.

En este caso usaremos el sistema operativo Tinycore. Este es una distribución de Linux, que debemos descargarla desde un repositorio de GitHub, ya que en su interior encontraremos un archivo que es el que generara nuestro DSM.

## Descarga de recursos

Descargaremos Tinycore desde el repositorio de GitHub:

[https://github.com/pocopico/tinycore-redpill/releases](https://github.com/pocopico/tinycore-redpill/releases)

Descargaremos el archivo que dice, **tinycore-redpill.v0.9.2.9.vmdk.gz,** este será el que necesitaremos para la máquina virtual ya que lo insertaremos como un disco duro más.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Github de descarga de Tinycore</p></figcaption></figure>

## Configuración VirtualBox

En el tipo de sistema debemos indicar **Linux**,  **Red Hat 64-bit**. Después para agilizar la instalación y velocidad del sistema recomiendo usar **2048 MB de RAM** y **2 procesadores**.

En las opciones de Almacenamiento en el **SATA0** debemos insertar el VMDK descargado anteriormente. Además, como mínimo debemos agregar dos discos duros virtuales con un **espacio mínimo de 20 GB**, para esta manera crear un **RAID 0**.

Por último, en las opciones de red debemos ponerlo en **Red NAT** o en **Adaptador Puente**, ya que se configuran desde el navegador web. Finalmente nos debe quedar de esta manera.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

## Preparación de instalación DSM

Ahora vamos a arrancar la máquina virtual, y esperar hasta que se inicie el sistema. Se nos abrián por defecto dos terminales, una de ellas vacía pero otro nos da información del sistema, uso de CPU, IP, etc. Para instalar DSM mas cómodamente nos conectaremos por SSH.&#x20;

Usuario: **tc**

Contraseña: **P@ssw0rd**

```
ssh tc@x.x.x.x
```

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### Primer paso - Actualizar rploader

Rploader es el script que se ejecutara a la hora de instalar nuestro DSM. Lo primero que tendremos que hacer es actualizarlo para tener la última versión a la hora de instalar DSM. A través del siguiente comando comprobaremos si tenemos la ultima versión, en el caso de que no nos solicitara actualizar.

```
sudo ./rploader.sh update
```

Para finalmente actualizar todos los arhivos ejecutaremos el siguiente comando, aunque no tengamos ninguna actualización.

```
sudo ./rploader.sh fullupgrade
```

### Segundo Paso - Visualización y elección de modelo de NAS

Esta herramienta cuenta con varios modelos de simulación de NAS, depende de que NAS elijamos tendremos acceso a unas características u otros. Dependiendo de nuestras necesidades elegiremos un modelo u otro, en este caso elegiremos el **DS3615xs**.

Para ver la lista con los NAS que hay ejecutamos el siguiente comando:

```
sudo ./rploader.sh serialgen
```

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption><p>Ver modelos de NAS</p></figcaption></figure>

Para seleccionar un dispositivo debemos introducir el siguiente comando y aceptar con la **“y”**:

```
sudo ./rploader.sh serialgen DS3615xs
```

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Generador de MAC</p></figcaption></figure>

En este paso es **muy importante** apuntar la dirección MAC que nos aparece ya que esta es diferente en cada instalación. En este caso será la:  **00:11:32:94:68:8A.** Si finalmente no la apuntamos la podemos encontrar en el archivo **user\_config.json.**

### **Tercer paso - Ver versiones de DSM y instalación**

Ya una vez seleccionado el modelo de NAS tenemos que elegir la versión que vamos a instalar. En el caso del modelo elegido (DS3615xs) debemos ir a buscar las versiones disponibles para ese modelo. Por lo general instalaremos la última versión, pero en este caso instalaremos una versión anterior a la más reciente para posteriormente explicar como se puede actualizarlo a la última versión.

Para ver las versiones disponibles ejecutamos el siguiente comando:

```
sudo ./rploader.sh
```

Para este caso instalaremos la versión **ds3615xs-7.1.0-42661**. Para ello debemos indicar el parámetro **build** seguidamente de la version. Al ejecutar el comando debemos aceptar la instalación.

```
sudo ./rploader.sh build ds3615xs-7.1.0-42661
```

<figure><img src="../../.gitbook/assets/Sin título.png" alt=""><figcaption><p>Selección de modelo ds3615xs-7.1.0-42661</p></figcaption></figure>

A partir de este momento comenzara la instalación, dependiendo de nuestra conexión ira más rápido o no, todos los archivos están en internet por lo que no podremos instalarlo sin conexión.

Seguidamente a través de un cliente de FTP accederemos a TinyCore para extraer el archivo de instalación que necesitaremos posteriormente. También podemos descargarlo desde la web oficial de Synology. El usuario y contraseña es el mismo que para acceder a SSH, el puerto es el 22, ya que la conexión se hace a través de sftp.

El archivo se encuentra en la siguiente ruta: **/home/tc/redpill-load/cache**

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Una vez extraído a nuestro equipo local el archivo de instalación apagaremos la máquina virtual con el siguiente comando:

```
exitcheck.sh poweroff
```

### Cuarto paso - Modificar MAC en VirtualBox

Una vez apagada la maquina virtual debemos acceder a la **configuración de red** de la maquina virtual y hacer clic en **Advanced**. Se abrirá un desplegable con diferentes opciones, entre ellas estará la **dirección MAC**. Debemos eliminar la MAC que aparece y copiar la que tenemos apuntada del **segundo paso**, pero sin ningún separador.

**¡¡HAY QUE RECORDAR QUE EN CADA INSTALACIÓN LA MAC ES DIFERENTE!!**

**00:11:32:94:68:8A -->00113294688A**

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
