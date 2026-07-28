---
description: raid
---

# mdadm

MDADM

Se trata de una herramienta en Linux que se utiliza para gestionar y administrar arrays de discos en RAID (Redundant Array of Independent Disks). Con `mdadm`, podemos crear, ensamblar, monitorear, y administrar RAID por software.

RAID es una tecnología que permite combinar varios discos duros en una sola unidad lógica para mejorar el rendimiento, la redundancia o ambas.

#### Funciones principales de `mdadm`:

1. **Crear RAID**: Configura diferentes tipos de RAID como RAID 0, RAID 1, RAID 5, RAID 6 y RAID 10, dependiendo de las necesidades de rendimiento o redundancia.
2. **Asamblear RAID**: Si un RAID ya existe, `mdadm` puede utilizarse para ensamblar y montar dicho array.
3. **Monitoreo de RAID**: Puede monitorear el estado de un RAID y enviar alertas en caso de fallos en los discos o degradación de la matriz.
4. **Administración de RAID**: Se pueden añadir o eliminar discos del RAID, reparar discos fallidos y reconstruir la matriz en caso de que un disco falle.

Vamos a crear un raid 1 a partir de una MV con Debian instalado. Para ello, le he añadido 3 hdd de 15GB cada uno y despertamos la máquina.

### Verificar los dispositivos instalados

Verificamos que los volúmenes en bloque existen.

```undefined
sudo lsblk
```

Este comando muestra información de todos los dispositivos de bloques disponibles en el sistema, como discos duros, particiones y unidades de almacenamiento. Por tanto, nos debe mostrar algo como lo siguiente:

<figure><img src="../../.gitbook/assets/image (850).png" alt="" width="322"><figcaption><p>Comprobando información de dispositivos</p></figcaption></figure>

### Trabajando con mdadm

Debemos comprobar si existe la herramienta mdadm en el sistema y si no la tenemos, instalamos:

```
sudo apt install mdadm
```

Para crear un RAID1 podemos utilizar el siguiente comando:

```
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
```

Si queremos crear un RAID1 con un disco de respuesto podemos hacer:

```undefined
sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc --spare-devices=1 /dev/sdd
```

* `--create:` crea un nuevo Raid
* `--level`: nivel de raid que necesitamos
* `--raid-devices`: número de dispositivos activos en la matriz
* `--spare-devices`: número de dispositivos de repuesto (extra) en la matriz inicial

Con este comando, el nuevo array se denomina `/dev/md0` y utiliza `/dev/sdb` y `/dev/sdc` para crear el RAID1. El dispositivo `/dev/sdd` se utiliza automáticamente como reserva para recuperarse del fallo de cualquier dispositivo activo.

<figure><img src="../../.gitbook/assets/image (851).png" alt="" width="375"><figcaption><p>Creación del raid1</p></figcaption></figure>

#### Comprobando

Para comprobar nuestro raid1 podemos ejecutar el siguiente comando:

```
mdadm --detail /dev/md0
```

Y nos mostrará algo como lo siguiente:

<figure><img src="../../.gitbook/assets/image (852).png" alt="" width="375"><figcaption><p>Verificando el raid1</p></figcaption></figure>

### Creando un sistema de archivos

Crearemos un sistema de archivos ext4 en el dispositivo RAID y lo vamos a montar. Para ello vamos a utilizar la herramienta mkfs.ext4

Este comando `mkfs.ext4` de Linux se utiliza para formatear dispositivos de almacenamiento, como discos duros, particiones o unidades USB. En este caso concreto, con el sistema de archivos **ext4** que es uno de los S.O más utilizados en Linux por su eficiencia, estabilidad y soporte para grandes volúmenes y archivos. Adicionalmente, ext4 ofrece mejoras en el manejo de grandes volúmenes de datos, mayor tolerancia a errores, así como mayor eficiencia en el uso del espacio.

```
sudo mkfs.ext4 -F /dev/md0
sudo mkdir /u01
sudo mount /dev/md0 /u01
```

Si queremos conocer el uso del disco del sistema de archivos podemos utilizar el comando. Por ejemplo:

<pre><code><strong>df -h
</strong></code></pre>

y nos mostrará el espacio en disco que está ocupando nuestro raid1.

<figure><img src="../../.gitbook/assets/image (853).png" alt="" width="375"><figcaption><p>df -h</p></figcaption></figure>

Ahora agregamos una entrada a _/etc/fstab_ y hacemos que el punto de montaje sea persistente tras los reinicios.

```
echo "/dev/md0    /data01    ext4    defaults    0 0" | sudo tee -a /etc/fstab > /dev/null
```

Podemos volver a hacer un revisión del estado del raid con:

```
mdadm --detail /dev/md0
```

Creando un archivo de configuración de Raid

Podemos agregar la configuración de RAID a `/etc/mdadm.conf`

El archivo de configuración identifica qué dispositivos son dispositivos RAID y a qué array pertenece un dispositivo específico. Según este archivo de configuración, `mdadm` puede ensamblar los array en el momento del inicio.

```
sudo mdadm --examine --scan | sudo tee -a /etc/mdadm.conf
```

Con el comando:

```
sudo mdadm --manage --help
```

podemos enumerar las opciones disponibles para gestionar un dispositivo RAID.

* `--add`: agrega en caliente dispositivos posteriores.
* `--remove`: elimine los dispositivos no activos posteriores.
* `--fail`: marca los dispositivos posteriores como defectuosos.

## <mark style="color:purple;">Trabajando con mdadm</mark>

Con estos comandos podemos controlar el estado de nuestro RAID.

**(1) Desmontar el sistema de archivos del RAID**

Una vez que conoces el punto de montaje, usa el comando `umount` para desmontarlo:

```bash
sudo umount /u01
```

#### **(2)** Detener un RAID

Nos aseguramos que el RAID no esté en uso. Para ello, desmontamos cualquier sistema de archivos que dependa del RAID. Después, podemos detener el arreglo con este comando:

```bash
sudo mdadm --stop /dev/md0
```

Nota: Recuerda que `/dev/md0` es el dispositivo del RAID. Este comando detendrá el dispositivo RAID especificado. Hay que asegurarse de que no se esté utilizando. Para ello, nos podemos asegurar de desmontar cualquier sistema de archivos montado en el RAID antes de detenerlo.

> **Nota**: No uses este comando si el RAID está en uso, ya que podría causar pérdida de datos o corrupción si hay procesos que dependan de él.

#### **(3)** Activar o Iniciar un RAID

Para volver a activar el RAID después de detenerlo, podemos utilizar el siguiente:

```bash
sudo mdadm --assemble --scan
```

Este comando buscará automáticamente los dispositivos RAID configurados y los montará. Si deseas activar un RAID específico, puedes especificar el dispositivo así:

```bash
sudo mdadm --assemble /dev/md0 /dev/sdb1 /dev/sdc1
```

Para volver a activar o iniciar el RAID después de haberlo detenido, también podemos hacer:

```bash
sudo mdadm --assemble --run /dev/md0
```

Esto ensamblará y activará el RAID especificado.

#### **Verificar el estado del RAID**

Si queremos verificar el estado del RAID:

```bash
cat /proc/mdstat
```

Esto mostrará el estado actual de todos los arrays RAID en el sistema, incluyendo si están activos, en proceso de reconstrucción, sincronización, etc.

> Nota: Recuerda tener precaución al realizar estas operaciones, especialmente si el RAID contiene datos importantes.
