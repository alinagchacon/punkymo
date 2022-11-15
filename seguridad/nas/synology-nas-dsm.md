---
description: Instalación de DSM sobre VirtualBox
---

# Synology NAS (DSM)

DSM es el sistema operativo que usan los NAS de la marca Synology. Este sistema es muy intuitivo y fácil de usar, además es muy útil a la hora de conectarse con diferentes usuarios. En este caso DSM lo instalaremos sobre otro sistema operativo, TinyCore de Redpill. Es una distribución de RedHat.

En este caso usaremos el sistema operativo Tinycore. Este es una distribución de Linux, que debemos descargarla desde un repositorio de GitHub, ya que en su interior encontraremos un archivo que es el que generara nuestro DSM.

## Descarga de recursos

Descargaremos Tinycore desde el repositorio de GitHub:

[https://github.com/pocopico/tinycore-redpill/releases](https://github.com/pocopico/tinycore-redpill/releases)

Descargaremos el archivo llamado, **tinycore-redpill.v0.9.2.9.vmdk.gz,** este será el que necesitaremos para la máquina virtual ya que lo insertaremos como un disco duro más.

<figure><img src="../../.gitbook/assets/image (1) (1) (2).png" alt=""><figcaption><p>Github de descarga de Tinycore</p></figcaption></figure>

## Configuración VirtualBox

En el tipo de sistema debemos indicar **Linux**,  **Red Hat 64-bit**. Después para agilizar la instalación y velocidad del sistema recomiendo usar **2048 MB de RAM** y **2 procesadores**.

En las opciones de Almacenamiento en el **SATA0** debemos insertar el VMDK descargado anteriormente. Además, como mínimo debemos agregar dos discos duros virtuales con un **espacio mínimo de 20 GB**, para esta manera crear un **RAID 0**.

Por último, en las opciones de red debemos ponerlo en **Red NAT** o en **Adaptador Puente**, ya que se configuran desde el navegador web. Finalmente nos debe quedar de esta manera.

<figure><img src="../../.gitbook/assets/image (5) (1) (2).png" alt=""><figcaption></figcaption></figure>

## Preparación de instalación DSM

Ahora vamos a arrancar la máquina virtual, y esperar hasta que se inicie el sistema. Se nos abrián por defecto dos terminales, una de ellas vacía pero otro nos da información del sistema, uso de CPU, IP, etc. Para instalar DSM mas cómodamente nos conectaremos por SSH.&#x20;

Usuario: **tc**

Contraseña: **P@ssw0rd**

```
ssh tc@x.x.x.x
```

<figure><img src="../../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (4) (3).png" alt=""><figcaption><p>Ver modelos de NAS</p></figcaption></figure>

Para seleccionar un dispositivo debemos introducir el siguiente comando y aceptar con la **“y”**:

```
sudo ./rploader.sh serialgen DS3615xs
```

<figure><img src="../../.gitbook/assets/image (8) (2).png" alt=""><figcaption><p>Generador de MAC</p></figcaption></figure>

En este paso es **muy importante** apuntar la dirección MAC que nos aparece ya que esta es diferente en cada instalación. En este caso será la:  **00:11:32:94:68:8A.** Si finalmente no la apuntamos la podemos encontrar en el archivo **user\_config.json.**

### **Tercer paso - Ver versiones de DSM y instalación**

Ya una vez seleccionado el modelo de NAS tenemos que elegir la versión que vamos a instalar. En el caso del modelo elegido (DS3615xs) debemos ir a buscar las versiones disponibles para ese modelo. Por lo general instalaremos la última versión, pero en este caso instalaremos una versión anterior a la más reciente para posteriormente explicar como se puede actualizarlo a la última versión.

Para ver las versiones disponibles ejecutamos el siguiente comando:

```
sudo ./rploader.sh
```

<figure><img src="../../.gitbook/assets/Sin título.png" alt=""><figcaption><p>Selección de modelo ds3615xs-7.1.0-42661</p></figcaption></figure>

Para este caso instalaremos la versión **ds3615xs-7.1.0-42661**. Para ello debemos indicar el parámetro **build** seguidamente de la versión. Al ejecutar el comando debemos aceptar la instalación.

```
sudo ./rploader.sh build ds3615xs-7.1.0-42661
```

A partir de este momento comenzara la instalación, dependiendo de nuestra conexión ira más rápido o no, todos los archivos están en internet por lo que no podremos instalarlo sin conexión.

Seguidamente a través de un cliente de FTP accederemos a TinyCore para extraer el archivo de instalación que necesitaremos posteriormente. También podemos descargarlo desde la web oficial de Synology. El usuario y contraseña es el mismo que para acceder a SSH, el puerto es el 22, ya que la conexión se hace a través de SFTP.

El archivo se encuentra en la siguiente ruta: **/home/tc/redpill-load/cache**

<figure><img src="../../.gitbook/assets/image (6) (2).png" alt=""><figcaption></figcaption></figure>

Una vez extraído a nuestro equipo local el archivo de instalación apagaremos la máquina virtual con el siguiente comando:

```
exitcheck.sh poweroff
```

### Cuarto paso - Modificar MAC en VirtualBox

Una vez apagada la maquina virtual debemos acceder a la **configuración de red** de la maquina virtual y hacer clic en **Advanced**. Se abrirá un desplegable con diferentes opciones, entre ellas estará la **dirección MAC**. Debemos eliminar la MAC que aparece y copiar la que tenemos apuntada del **segundo paso**, pero sin ningún separador.

**¡¡HAY QUE RECORDAR QUE EN CADA INSTALACIÓN LA MAC ES DIFERENTE!!**

**00:11:32:94:68:8A -->00113294688A**

<figure><img src="../../.gitbook/assets/image (3) (1) (2).png" alt=""><figcaption></figcaption></figure>

Una vez ya la hemos modificado, aceptamos los cambios para que se guarden y arrancaremos la máquina virtual.

En el menú de arranque debemos seleccionar la segunda opción, la que dice **RedPill DS3615xs v7.0.1-42661 (SATA, Verbose)**. Una vez que lo elijamos la primera vez se quedara marcada como opción predeterminada.

<figure><img src="../../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

### Quinto paso - Encontrar el NAS en la red

Ya encendida la maquina y esperado alrededor de unos 30 segundos debería estar operativo el NAS. Para encontrarlo en nuestra red debemos hacerlo a través una web de Synology. Accediendo a este enlace se iniciará la búsqueda del NAS: [https://finds.synology.com/](https://finds.synology.com/)

> En este manual, el NAS podrá tener varias direcciones IP diferentes ya que este se hace desde diferentes redes.

Nos debe aparecer una ventana como la de la siguiente imagen, en el caso de no aparecer, revisar que la MAC este correctamente escrita:

<figure><img src="../../.gitbook/assets/image (4) (2).png" alt=""><figcaption><p>Encontrar Synology NAS</p></figcaption></figure>

Después haremos clic en **“Conectar”** y a continuación aceptaremos los términos y condiciones que se indican. Al aceptarlos se nos redirigirá a la interfaz web del propio NAS para comenzar con la instalación**.**&#x20;

Seguidamente nos saldrá otra ventana donde haremos clic en **"Instalar"** y si queremos información del dispositivo haremos clic en el botón de justo debajo.&#x20;

### **Sexto paso - Archivo de instalación**

A continuación, nos indica que carguemos el archivo .pat. Este es el que hemos extraído del servidor a través de FTP. Debemos buscarlo en nuestro equipo y cargarlo.

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

Después de eso debemos confirmar que todos los archivos serán eliminados y comenzará la instalación.

Al acabar se reiniciará automáticamente, no debemos interrumpir este proceso. Nos saldará una ventana con un temporizador de 10 minutos. Este es el tiempo que tardará aproximadamente en terminar todo el proceso de instalación, aunque pasados un par de minutos podemos recargar la página para ver si ya ha terminado, ya que a veces no se carga automáticamente.

> Nota: A veces puede ser que al reiniciarse el NAS, el servidor DHCP asigne una dirección IP diferente. Para saber que nueva dirección IP ha asignado podemos volver a la web de búsqueda del NAS ([https://finds.synology.com/](https://finds.synology.com/)) para saberla. Puede ser que al acceder a la web de búsqueda no aparezca, en ese caso descargar la aplicación **Synology Assistant**, podeis hacer clic en este [enlace](https://finds.synology.com/helpers/assistant.php?platform=Windows\&product=).

### Séptimo paso - Iniciar configuración inicial

Al acabar de reiniciarse nos encontraremos con la página de inicio para la configuración, una página como la que aparece a continuación:

<figure><img src="../../.gitbook/assets/image (2) (1) (2).png" alt=""><figcaption></figcaption></figure>

Al hacer clic en Iniciar nos saldrá otra ventana para rellenar el nombre del dispositivo, nombre de la cuenta de administrador, contraseña y una casilla para permitir que nuestro NAS se muestra al usar el Web Assistant (la web de encontrar el NAS). Una vez lo rellenemos todo haremos clic en **"Siguiente"**.

<figure><img src="../../.gitbook/assets/image (10) (2).png" alt=""><figcaption></figcaption></figure>

Ahora nos mostrara una ventana para elegir de qué manera se actualiza el DSM y los paquetes, es obligatorio poner la última opción, la actualización manual. Si el NAS se actualiza de forma automática no se volverá a encender hasta que no sigamos un procedimiento especifico, por lo tanto actualizaremos el NAS de forma manual.

<figure><img src="../../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

En el siguiente paso nos indica si queremos crear una cuenta de Synology para tener acceso a más servicios. Como este NAS no está registrado en Synology no podremos acceder a estos servicios, por lo tanto, debemos omitir este paso.

<figure><img src="../../.gitbook/assets/image (5) (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

Por último paso nos pide permiso para recopilar datos o no, es opcional marcar esta casilla ya que no influirá en el funcionamiento del NAS.

<figure><img src="../../.gitbook/assets/image (8) (3).png" alt=""><figcaption></figcaption></figure>

Una vez hecho todo este proceso ya nos encontraremos con la pantalla principal de nuestro NAS.

<figure><img src="../../.gitbook/assets/image (11) (2).png" alt=""><figcaption></figcaption></figure>
