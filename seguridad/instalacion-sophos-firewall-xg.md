---
description: JLM
---

# Instalación Sophos Firewall XG

## ¿Que es SOPHOS Firewall?

Es un firewall de la compañía Sophos. Son Firewalls pensados para empresas ya que cuenta con muchas opciones de seguridad. Además, tiene diferentes versiones de Firewall. Tiene una versión llamada Sophos Home pensada para hogares y pequeñas. Después nos encontramos con el Sophos Firewall XG que está pensada para empresas y con más opciones de seguridad y protección.

Ahora instalaremos el Sophos Firewall XG en su versión Home, esta nos permite usar funciones limitadas, pero podemos usarlo de forma ilimitada. Para poder probar sus funciones y conocer su funcionamiento usaremos una máquina virtual que nos ofrece Sophos.

Tambien tenemos la opción de instalarlo a través de una ISO de forma manual en un equipo físico, el unico requisito destacable es que tenga 4 Gb de RAM.

### Descarga VM Sophos Firewall

Para descargar SOPHOS Firewall debemos hacerlo desde esta URL: [https://www.sophos.com/en-us/support/downloads/firewall-installers](https://www.sophos.com/en-us/support/downloads/firewall-installers). Después tenemos que elegir la opción de **Virtual Installers: Firewall OS for VMware**.

<figure><img src="../.gitbook/assets/image (9) (3) (1).png" alt=""><figcaption><p>Descarga sophos firewall</p></figcaption></figure>

Antes de la descarga debemos de rellenar ciertos datos que pueden ser aleatorios, no es necesario poner nuestro correo real.

<figure><img src="../.gitbook/assets/image (7) (1) (2) (2).png" alt=""><figcaption><p>Términos y condiciones de descarga</p></figcaption></figure>

Al enviar el formulario comenzará la descarga del Firewall, este se descargará en un zip que posteriormente descomprimiremos.

Después debemos de acceder a otra web para poder solicitar el número de serie del firewall para su posterior activación. Esta licencia sirve para usar el firewall en su versión “home” con validez de por vida. Esta licencia no podremos usarla en un entorno empresarial.

La web que debemos acceder es la siguiente: [https://www.sophos.com/es-es/products/free-tools/sophos-xg-firewall-home-edition/software](https://www.sophos.com/es-es/products/free-tools/sophos-xg-firewall-home-edition/software)

<figure><img src="../.gitbook/assets/image (14) (1) (3).png" alt=""><figcaption><p>Solicitud de número de serie</p></figcaption></figure>

En este caso si debemos de poner nuestro correo real ya que es ahí donde recibiremos el número de serie, es opcional poner nuestro nombre y apellidos reales. Es muy importante no descargar el archivo ISO que nos aparece al enviar el formulario, usaremos el zip anterior descargado.

### Configuración VirtualBox

Cuando se descarga debemos descomprimirlo y hacer doble clic en **sf\_virtual\_virtualbox.ovf**. Esto hará que se importe la máquina virtual a VirtualBox y podremos elegir la ubicación donde queremos que se importe. En el caso de que no se importe nos vamos a la pantalla principal de virtualbox, le damos a **añadir** y buscamos el archivo dicho anteriormente y lo seleccionamos y empezara la importación.

<figure><img src="../.gitbook/assets/image (9) (1) (2).png" alt=""><figcaption></figcaption></figure>

Antes de iniciar la maquina debemos de hacer ciertas configuraciones a la red, debemos de poner en el Adaptador 1 en Red Interna **(itnetAdmin)**, el Adaptador 2 ponerlo en red interna, pero en otra diferente **(itnetUsers)**, el tercer adaptador en red interna pero también en otra diferente **(itnetDMZ)** y el cuarto adaptador en **NAT)** Nos debe de quedar de esta manera:

Las redes internas se van creando a medida que se escriben.

<figure><img src="../.gitbook/assets/image (2) (3).png" alt=""><figcaption><p>Configuración redes en VirtualBox</p></figcaption></figure>

Una vez hecho esto ya podremos arrancar la máquina virtual. No debemos tocar la selección de sistema del GRUB.

### Configuración desde la interfaz de la VM

Una vez arrancamos la máquina virtual se comenzarán a iniciar todos los procesos y la configuración inicial.

Seguidamente nos pedirá la contraseña para acceder, de forma predeterminada es **admin**.

<figure><img src="../.gitbook/assets/image (3) (1) (4).png" alt=""><figcaption></figcaption></figure>

Después nos aparecerá un mensaje sobre los Términos de Licencia del firewall, si queremos seguir utilizando debemos aceptarlos presionando la tecla **A**.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

Una vez lo aceptamos nos aparecerá la pantalla principal del Firewall. Debe ser como la siguiente:

<figure><img src="../.gitbook/assets/image (11) (2).png" alt=""><figcaption></figcaption></figure>

Desde la propia interfaz de Sophos no realizaremos ninguna configuración, esta se hace desde el navegador.

### Acceder desde el navegador

Para poder entrar al panel de administración y poder configurarlo debemos abrir otra máquina virtual con cualquier sistema operativo y configurar la red en la Red Interna de **itnetAdmin**. Después abrimos cualquier navegador web y en la barra URL escribir la dirección IP por defecto de Sophos que es la **172.16.16.16**. Es importante acceder a través de https e indicando el puerto especifico que será el 4444. La URL en este caso quedaría de esta forma: [https://172.16.16.16:4444](https://172.16.16.16:4444)

Cuando accedemos nos saldrá una advertencia sobre la conexión, detecta que no tiene certificado SSL y por lo tanto nos avisa que la conexión no es segura. Para saltar este error debemos de hacer clic en **Avanzado** y en **Continuar (dirección\_ip)**.

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Si todo ha funcionado correctamente nos aparecerá la página de bienvenida, está la podemos ver en la siguiente imagen debe de aparecer una página como esta:

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

En el caso de que no aparecer esta página, y apareciera directamente un Login debemos abrir otra pestaña y volver a escribir la URL.

Para continuar con la configuración seleccionamos la casilla de los Términos y condiciones de Sophos Firewall y hacemos clic en **Start Setup**. Si antes de continuar queremos cambiar el idioma podemos abrir el desplegable donde pone **English** y seleccionar **Spanish** para mayor comodidad de lectura.

## Configuración inicial desde la web

Ya finalizados los pasos previos debemos de comenzar el proceso de configuración principal. Este nos permitirá establecer las primeras configuraciones de uso del firewall y activarlo.

### Primer paso - Establecer contraseña de administrador

Lo primero que debemos hacer es seleccionar una contraseña segura con las características que nos muestra por pantalla, ya que si no nos dejara continuar.

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption><p>Establecer contraseña</p></figcaption></figure>

La casilla de **Instalar el firmware más reciente** es recomendable que este seleccionada.

### Segundo paso - Conexión a internet

Ahora debemos establecer el adaptador de red por el cual ira la conexión internet. Al estar tener varios adaptadores en red interna es posible que el Firewall no detecta cual es el puerto que tiene salida a Internet. Por eso no sale un error como el que podemos ver a continuación:

<figure><img src="../.gitbook/assets/image (2) (2) (2).png" alt=""><figcaption><p>Error de conexión a internet</p></figcaption></figure>

Para solucionar este problema hacemos clic en el botón de **Manual configuration** y debemos abrir el desplegable de **Choose a port to configure** y seleccionar el **Port D** ya que este es el adaptador que tiene salida a Internet en NAT. Una vez le demos a **Apply** tardará unos segundos en configurarse.

<figure><img src="../.gitbook/assets/image (4) (1) (4).png" alt=""><figcaption><p>Cambiar adaptador de internet</p></figcaption></figure>

Después se debe quitar el error y aparecernos que tenemos internet como podemos ver en la siguiente imagen. Una vez hecho esto hacemos clic en **Continue**.

