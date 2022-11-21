---
description: JLM
---

# Creación de volúmenes y su uso

En esta sección crearemos un grupo de almacenamiento y un volumen, estos son necesarios ya que desde estos se gestiona todas las carpetas y datos que almacenemos en el NAS.

Un grupo de almacenamiento es donde agruparemos las unidades de disco para crear los RAID. Estas agrupaciones pueden ser de 1 o de varias unidades.

Por cada grupo de almacenamiento crearemos los volúmenes, estos son los que nos proporcionaran el almacenamiento a nuestro NAS a través de la creación de carpetas.

## Crear grupo de almacenamiento

Para comenzar primero debemos crear el grupo de almacenamiento con el RAID y después el volumen con el espacio que asignaremos. En este caso crearemos un RAID 0 con los discos duros de 20 GB que tenemos insertados.

### Primer paso - Abrir administrador de almacenamiento

Para acceder a al administrador de almacenamiento, debemos situarnos en el icono, de la esquina superior izquierda, este botón nos muestra todas las aplicaciones de nuestro NAS, muchas de ellas ya vienen de forma predeterminada. Entre ellas accederemos al administrador de almacenamiento.

<figure><img src="../../../.gitbook/assets/image (1) (3).png" alt=""><figcaption><p>Inicio de adminsitrador de almacenamiento</p></figcaption></figure>

Para comenzar la creación de el grupo de almacenamiento y el voluemn hacemos clic en iniciar.

### Segundo paso - Selección de tipo de RAID

Ahora debemos seleccionar el RAID que queremos hacer con nuestras unidades de almacenamiento. Si vamos seleccionando en el desplegable entre los distintos tipos de RAID que nos ofrece podemos ver un pequeña descripción de lo que significa cada uno. En este caso seleccionaremos el RAID 0, este nos permitirá tener mas espacio en los discos y mayor velocidad.

De forma opcional podemos agregarle una descripción para poder identificarlo, introduciré: **Mi primer grupo de almacenamiento**.

<figure><img src="../../../.gitbook/assets/image (6) (2).png" alt=""><figcaption><p>Selección de RAID</p></figcaption></figure>

### Tercer paso - Selección de discos para el grupo de almacenamiento

Al hacer clic en Siguiente, nos encontraremos una ventana donde debemos elegir las unidades que formaran este grupo de almacenamiento, en este caso únicamente tenemos dos unidades, para seleccionarlas debemos desplazarlas hacia el panel derecho.

<figure><img src="../../../.gitbook/assets/image (3) (1) (3).png" alt=""><figcaption><p>Selección de discos</p></figcaption></figure>

### Cuarto paso - Comprobación de la unidad

Al hacer clic en Siguiente, nos encontramos con una ventana donde nos da a alegir entre hacer una comprobación de la unidad o no. En este caso no haremos ninguna comprobación de la unidades ya que es un entorno de pruebas, en un caso real recomiendo hacerla para evitar futuros errores.

<figure><img src="../../../.gitbook/assets/image (5) (1) (2) (2).png" alt=""><figcaption><p>Comprobación de la unidad</p></figcaption></figure>

### Quinto paso - Asiganación de capacidad al volumen

A **continuación** encontramos la ventana para asignar la capacidad deseada a nuestro volumen. Durante la creación de RAID siempre se reduce el espacio total ya que se reserva un porcentaje de almacenamiento para que funcione correctamente. En este caso la máxima capacidad que podrá tener nuestro RAID será de **19 GB**. Si no queremos dedicar todo el espacio podemos indicar menos espacio, respteando el rango que nos indica al pasar por encima de la casilla.

De forma opcional podemos agregarle una descripción para poder identificarlo, introduciré: **Mi primer volumen**

<figure><img src="../../../.gitbook/assets/image (2) (2).png" alt=""><figcaption><p>Asignación de capacidad al volumen</p></figcaption></figure>

### Sexto paso - Seleccionar un sistema de archivos

En el siguiente paso nos encontraremos la selección del sistema de archivos, de forma recomendada usaremos **btrfs**, este cuenta con funciones avanzadas frente a **ext4**. Además las ventajas de migración de sistemas con versioens anteriores de NAS no lo usaremos.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption><p>Selección sistema de archivos </p></figcaption></figure>

### Séptimo paso - Revisión de la configuración

Por ultimo paso debemos revisar que toda la configuración este correcta, si es que si podemos darle a **Aplicar**, en el caso de no ser correcta podemos volver hacia atrás y cambiar los datos que no estén correctos.

Una vez apliquemos los cambias nos saltara una alerta que nos dice que todos los datos serán eliminados, si estamos seguros le decimos que **OK**.

<figure><img src="../../../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption><p>Revisión de configuración</p></figcaption></figure>

Una vez ya creado podemos ver como es el panel de nuestro grupo de almacenamiento y volumen. Como podemos ver el espacio del disco duro se queda bastante reducido.

<figure><img src="../../../.gitbook/assets/image (3) (4).png" alt=""><figcaption><p>Panel principal del administrador de almacenamiento</p></figcaption></figure>

## Uso del administrador de almacenamiento

Este aplicación además de ofrecernos información acerca del espacio disponible en nuestro volumen nos informa del estado de nuestras unidades.

En el panel lateral izquierdo, en Visión general podemos ver una visión mas general y poco detallada acerca del almacenamiento, aunque la ultima sección podemos ver las tareas programadas que tiene el disco. En este caso tiene una prueba S.M.A.R.T.

<figure><img src="../../../.gitbook/assets/image (7) (1) (2) (1).png" alt=""><figcaption><p>Visión general</p></figcaption></figure>

En el ultimo apartado (HDD/SSD) del panel derecho podemos ver la información técnica de los discos duros insertados en el NAS. Aquí nos puede mostrar información como, la temperatura de los discos, horas encendido, sectores defectuosos, etc.&#x20;

Y si hacemos clic en Información de salud aun podemos ver mas información acerca del disco.

<figure><img src="../../../.gitbook/assets/image (9) (2).png" alt=""><figcaption><p>Información de los discos duros</p></figcaption></figure>
