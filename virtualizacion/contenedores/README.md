---
description: N-Tech Admin Group
---

# Contenedores

Cuando hablamos de virtualización tenemos que pensar en una simulación o emulación de un entorno de software, ya sea un sistema operativo (SO) o una aplicación o conjunto de aplicaciones.&#x20;

En la virtualización se separa el hardware (recursos físicos) para crear sistemas independientes, los unos de los otros como máquinas virtuales (MV) o contenedores (LXC, Docker).

El encargado de ejecutar una MV o contenedor se denomina <mark style="color:blue;">`Hipervisor`</mark>. Son ellos los que asignan aspectos básicos como son la CPU, RAM, el almacenamiento, etc.

### Hipervisores

Los hipervisores crean una capa de virtualización que separa la CPU / procesadores , la RAM y otros recursos físicos de las MV.

Los hipervisores emulan los recursos disponibles para que las máquinas invitadas puedan usarlos. Independientemente del SO que inicie con una MV, pensará que el hardware físico real está a su disposición.

Desde el punto de vista de una MV, no hay diferencia entre el entorno físico y el virtualizado. Las máquinas invitadas no saben que el hipervisor las creó en un entorno virtual y que comparten la potencia de cómputo disponible. Dado que las máquinas virtuales se ejecutan simultáneamente con el hardware que las alimenta, dependen completamente de su funcionamiento estable.

En la arquitectura de la nube, las máquinas virtuales y los contenedores sin un pilar fundamental a la hora de ejecutar servicios. Una de las ofertas estándar de [Azure](https://azure.microsoft.com/es-es/), [Google Cloud ](https://cloud.google.com/gcp/?hl=es\&utm\_source=google\&utm\_medium=cpc\&utm\_campaign=emea-es-all-es-bkws-all-all-trial-e-gcp-1011340\&utm\_content=text-ad-none-any-DEV\_c-CRE\_495030365273-ADGP\_Hybrid%20%7C%20BKWS%20-%20EXA%20%7C%20Txt%20\~%20GCP%20\~%20General%23v3-KWID\_43700060384861663-kwd-6458750523-userloc\_1005424\&utm\_term=KW\_google%20cloud-NET\_g-PLAC\_\&gclid=EAIaIQobChMIxIaPjdf5-gIVAdZ3Ch10-QQ6EAAYASAAEgLKMfD\_BwE\&gclsrc=aw.ds)y [AWS](https://aws.amazon.com/es/free/?trk=2d5aad89-991b-4184-98b5-1f562e3102c8\&sc\_channel=ps\&s\_kwcid=AL!4422!3!561218200770!e!!g!!aws\&ef\_id=EAIaIQobChMIvqb7ntf5-gIVAvd3Ch07UAHlEAAYASAAEgJ71fD\_BwE:G:s\&s\_kwcid=AL!4422!3!561218200770!e!!g!!aws\&all-free-tier.sort-by=item.additionalFields.SortRank\&all-free-tier.sort-order=asc\&awsf.Free%20Tier%20Types=\*all\&awsf.Free%20Tier%20Categories=\*all) son precisamente las MV.

Existen dos tipos de hipervisores:&#x20;

* tipo 1 bare-metal o nativo.
* tipo 2. hipervisores alojados.

### **Tipo 1**

No necesita SO sino que se ejecuta en su lugar, lo que reduce considerablemente los recursos que se le dan al host. Se suele usar para los servidores, ya que no es necesario tener un SO debajo del hipervisor que ocupe recursos inútilmente. \
Es, por tanto, una capa de software que instalamos directamente sobre un servidor físico y su hardware subyacente. No hay software ni ningún sistema operativo en el medio. Ejemplos de este tipo de hipervisor tenemos <mark style="color:blue;">Hyper-V</mark>, <mark style="color:blue;">`ESXi`</mark>, <mark style="color:blue;">Proxmox</mark> entre otros.

### **Tipo 2**

Es el hipervisor ejecutado en un SO, ya sea Windows o Linux como si fuera un programa más. Suele ser el tipo más común, ya que para uso personal es la mejor opción, porque tienes tu SO host y tus VM.  Ejemplos de este tipo de hipervisor son los dos más populares <mark style="color:blue;">VMWare</mark> y <mark style="color:blue;">VirtualBox</mark>.&#x20;

Por tanto, la virtualización de tipo 2 se caracteriza porque debe ser instalado en un equipo que cuente con un SO previo como Ubuntu, Debian, Windows,  Mac, etc.

La principal diferencia entre un hipervisor de tipo 1 y uno de tipo 2 es el sistema operativo host subyacente que solo está presente en el hipervisor de tipo 2. &#x20;

### Máquinas virtuales

Las máquinas virtuales MV simulan un sistema informático que puede ejecutar programas como si fuese un equipo real. Los procesos que ejecutan las MV están limitados por los recursos proporcionados por el entorno real que las contiene.

Dado que una MV emula un SO con sus propios sistemas operativos, para administrar y proporcionar el servicio de MV a los usuarios finales, se necesitan hipervisores capaces de ejecutar varias MV en una infraestructura compartida.&#x20;

Toda la simulación se encapsula en una serie de archivos que actúan como contenedor desde el que se ejecutará la máquina virtual en una ventana de nuestro ordenador como cualquier otro programa y sin el peligro de que lo que suceda a la MV acabe afectando a nuestro equipo real donde se está ejecutando.

### Contenedores

Los contenedores solo requieren los componentes del SO necesarios para ejecutar la aplicación. Por lo general, Linux y Windows se utilizan como SO que se comunican directamente con Container Engine. Cada contenedor comparte el kernel del sistema operativo host y, por lo general, también los archivos binarios y las bibliotecas. Para el uso eficiente de los recursos, los componentes compartidos son de solo lectura, lo que reduce el tamaño del contenedor y aumenta el tiempo de inicio. &#x20;

Algunas de las ventajas que puede traer el uso de los contenedores son:&#x20;

* evitar que se produzcan bloqueos por entornos incompatibles y uniformizar el rendimiento en todos los equipos.&#x20;
* Permitir a los desarrolladores que puedan enfocarse en la propia aplicación y no en la corrección de errores o en tener que reescribir la aplicación para diferentes entornos de servidores.&#x20;
* Al no tener SO, permiten ofrecer una manera eficiente de que los desarrolladores puedan desplegarlos en clúster, donde los contenedores individuales contienen componentes únicos de aplicaciones complejas.&#x20;
* Al dividir los componentes en contenedores separados, los desarrolladores también pueden actualizar componentes individuales en lugar de reprocesar toda la aplicación.

Otras ventajas más llamativas por así decirlo son:

* Tamaño: los contenedores solo tienen unas decenas de megas.
* Modularidad: se pueden dividir en módulos más pequeños.
* Velocidad: Los contenedores pueden ejecutarse casi al instante.
* Autosuficiencia: Las aplicaciones se ejecutan virtualmente dentro de sus propios contenedores pequeños.
* Portabilidad: Los contenedores operan en cualquier entorno.

<figure><img src="../../.gitbook/assets/image (146).png" alt=""><figcaption><p>Tomado de <a href="https://mkaschke.medium.com/virtual-machine-vm-vs-container-13ab51f4c177">https://mkaschke.medium.com/virtual-machine-vm-vs-container-13ab51f4c177</a></p></figcaption></figure>

Podemos resumir entonces que:

* Un contenedor es un conjunto de uno o más procesos separados del resto del sistema.
* Todos los archivos que se necesitan vienen de una imagen diferente, lo que significa que son portátiles y uniformes durante todas las etapas: desarrollo, prueba y producción.
* Son mucho más rápidos a la hora de replicar entornos de prueba tradicionales.
* Son parte importante en la seguridad de TI.
*   Comparten el mismo kernel del sistema operativo y separan los procesos de las aplicaciones del resto del sistema.&#x20;

    \
    **Ejemplo**:
* los sistemas Linux ARM ejecutan contenedores de Linux ARM,&#x20;
* los sistemas Linux x86 ejecutan contenedores de Linux x86,&#x20;
* los sistemas Windows x86 ejecutan contenedores de Windows x86.&#x20;
* los contenedores de Linux son muy portátiles, pero deben ser compatibles con el sistema subyacente.

### LXC LinuX Containers <a href="#bkmrk-page-title" id="bkmrk-page-title"></a>

LXC o linuX Containers son contenedores aislados con su propia reserva de recursos y se realizan mediante entornos de virtualización. Los contenedores no emulan hardware, algo que si hacen las máquinas virtuales.&#x20;

LXC es un sistema operativo dentro de nuestro sistema y a efectos prácticos se comporta como una MV, la emulación se ejecuta en el kernel de linux. Pero **LXC** proporciona un **contenedor** mínimo, así reduce el uso de recursos de nuestro sistema y aumenta la portabilidad gracias a que se pueden crear plantillas que se pueden ejecutar en cualquier contenedor.

Se pueden crear los contenedores de dos maneras:

* Por terminal
* Por interfaz gráfica (como es el caso de Proxmox)

#### Terminal <a href="#_rv7olwu9z7el" id="_rv7olwu9z7el"></a>

`lxc-create -n MyCentOSContainer1 -t /usr/local/share/lxc/templates/lxc-centos/usr/local/share/lxc/templates/lxc-centos`

### Máquinas virtuales vs LXC

La gran diferencia entre las dos es la versatilidad y cómo usan los recursos. A la hora de desplegar plataformas autónomas y aisladas la máquina virtual tendrá que instalar todo el software y componentes requeridos para la herramienta pero con los contenedores LXC pueden agruparse todos los recursos e instalarse todas las veces que sea necesario y de forma autónoma.

<figure><img src="../../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>

Por otra parte, Docker o los contenedores de aplicaciones al utilizar el sistema operativo anfitrión y solo poder usar una aplicación por contenedor requiere menos recursos del sistema y es más fácil de trasladar.

Los inconvenientes de Docker y por la cual PROXMOX usa LXC es que Docker está limitado a usar una aplicación lo que significa que si, por ejemplo, queremos tener un servidor web en un contenedor, en LXC crearemos un contenedor con el sistema operativo que fuera más conveniente e instalaríamos todos los programas necesarios como Apache, MySQL, WordPress entre otros.

En Docker todos los servicios tendrían que estar en diferentes contenedores, uno para Apache, otro para MySQL y así con todos los procesos. Esto hace de LXC la mejor opción para contenedores de uso empresarial, donde se hace necesario usar varios procesos juntos.

### Links

* [https://www.hpe.com/es/es/what-is/containers.html#:\~:text=Los%20contenedores%20son%20tecnología%20que,sistema%20operativo%20en%20cualquier%20contexto.](https://www.hpe.com/es/es/what-is/containers.html)&#x20;
