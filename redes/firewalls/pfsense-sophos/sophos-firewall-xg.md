---
description: Joel López Molina
---

# Sophos Firewall XG

## ¿Que es SOPHOS Firewall?

Es un firewall de la compañía Sophos. Son Firewalls pensados para empresas ya que cuenta con muchas opciones de seguridad. Además, tiene diferentes versiones de Firewall. Tiene una versión llamada Sophos Home pensada para hogares y pequeñas. Después nos encontramos con el Sophos Firewall XG que está pensada para empresas y con más opciones de seguridad y protección.

Ahora instalaremos el Sophos Firewall XG en su versión Home, esta nos permite usar funciones limitadas, pero podemos usarlo de forma ilimitada. Para poder probar sus funciones y conocer su funcionamiento usaremos una máquina virtual que nos ofrece Sophos.

También tenemos la opción de instalarlo a través de una ISO de forma manual en un equipo físico, el unico requisito destacable es que tenga 4 Gb de RAM.

### Descarga VM Sophos Firewall

Para descargar SOPHOS Firewall debemos hacerlo desde esta URL: [https://www.sophos.com/en-us/support/downloads/firewall-installers](https://www.sophos.com/en-us/support/downloads/firewall-installers). Después tenemos que elegir la opción de **Virtual Installers: Firewall OS for VMware**.

<figure><img src="../../../.gitbook/assets/image (26) (1).png" alt=""><figcaption><p>Descarga Sophos Firewall</p></figcaption></figure>

Antes de la descarga debemos de rellenar ciertos datos que pueden ser aleatorios, no es necesario poner nuestro correo real.

<figure><img src="../../../.gitbook/assets/image (19) (1) (2).png" alt=""><figcaption><p>Terminos y condiciones de descarga</p></figcaption></figure>

Al enviar el formulario comenzará la descarga del Firewall, este se descargará en un zip que posteriormente descomprimiremos.

Después debemos de acceder a otra web para poder solicitar el número de serie del firewall para su posterior activación. Esta licencia sirve para usar el firewall en su versión “home” con validez de por vida. Esta licencia no podremos usarla en un entorno empresarial.

La web que debemos acceder es la siguiente: [https://www.sophos.com/es-es/products/free-tools/sophos-xg-firewall-home-edition/software](https://www.sophos.com/es-es/products/free-tools/sophos-xg-firewall-home-edition/software)

<figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption><p>Solicitar número de serie</p></figcaption></figure>

En este caso si debemos de poner nuestro correo real ya que es ahí donde recibiremos el número de serie, es opcional poner nuestro nombre y apellidos reales. Es muy importante no descargar el archivo ISO que nos aparece al enviar el formulario, usaremos el zip anterior descargado.

### Configuración VirtualBox

Cuando se descarga debemos descomprimirlo y hacer doble clic en **sf\_virtual\_virtualbox.ovf**. Esto hará que se importe la máquina virtual a VirtualBox y podremos elegir la ubicación donde queremos que se importe. En el caso de que no se importe nos vamos a la pantalla principal de VirtualBox, le damos a **añadir** y buscamos el archivo dicho anteriormente y lo seleccionamos y empezara la importación.

<figure><img src="../../../.gitbook/assets/image (18) (1) (3).png" alt=""><figcaption><p>Importación VM Sophos</p></figcaption></figure>

Antes de iniciar la maquina debemos de hacer ciertas configuraciones a la red, debemos de poner en el Adaptador 1 en Red Interna **(itnetAdmin)**, el Adaptador 2 ponerlo en red interna, pero en otra diferente **(itnetUsers)**, el tercer adaptador en red interna pero también en otra diferente **(itnetDMZ)** y el cuarto adaptador en **NAT)** Nos debe de quedar de esta manera:

Las redes internas se van creando a medida que se escriben.

<figure><img src="../../../.gitbook/assets/image (53).png" alt=""><figcaption><p>Configuración adicional Sophos</p></figcaption></figure>

Una vez hecho esto ya podremos arrancar la máquina virtual. No debemos tocar la selección de sistema del GRUB.

### Configuración desde la interfaz de la VM

Una vez arrancamos la máquina virtual se comenzarán a iniciar todos los procesos y la configuración inicial.

Seguidamente nos pedirá la contraseña para acceder, de forma predeterminada es **admin**.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Interfaz de VM</p></figcaption></figure>

Después nos aparecerá un mensaje sobre los Términos de Licencia del firewall, si queremos seguir utilizando debemos aceptarlos presionando la tecla **A**.

<figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption><p>Aceptar terminos y condiciones</p></figcaption></figure>

Una vez lo aceptamos nos aparecerá la pantalla principal del Firewall. Debe ser como la siguiente:

<figure><img src="../../../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption><p>Pantalla principal de Sophos</p></figcaption></figure>

Desde la propia interfaz de Sophos no realizaremos ninguna configuración, esta se hace desde el navegador.

### Acceder desde el navegador

Para poder entrar al panel de administración y poder configurarlo debemos abrir otra máquina virtual con cualquier sistema operativo y configurar la red en la Red Interna de **itnetAdmin**. Después abrimos cualquier navegador web y en la barra URL escribir la dirección IP por defecto de Sophos que es la **172.16.16.16**. Es importante acceder a través de https e indicando el puerto especifico que será el 4444. La URL en este caso quedaría de esta forma: [https://172.16.16.16:4444](https://172.16.16.16:4444)

Cuando accedemos nos saldrá una advertencia sobre la conexión, detecta que no tiene certificado SSL y por lo tanto nos avisa que la conexión no es segura. Para saltar este error debemos de hacer clic en **Avanzado** y en **Aceptar el riesgo y continuar**.

<figure><img src="../../../.gitbook/assets/image (33).png" alt=""><figcaption><p>Advertencia de seguridad del navegador</p></figcaption></figure>

Si todo ha funcionado correctamente nos aparecerá la página de bienvenida, está la podemos ver en la siguiente imagen debe de aparecer una página como esta:

En el caso de que no aparecer esta página, y apareciera directamente un Login debemos abrir otra pestaña y volver a escribir la URL.

Para continuar con la configuración seleccionamos la casilla de los Términos y condiciones de Sophos Firewall y hacemos clic en **Start Setup**. Si antes de continuar queremos cambiar el idioma podemos abrir el desplegable donde pone **English** y seleccionar **Spanish** para mayor comodidad de lectura.

## Configuración inicial desde la web

Ya finalizados los pasos previos debemos de comenzar el proceso de configuración principal. Este nos permitirá establecer las primeras configuraciones de uso del firewall y activarlo.

### Primer paso - Establecer contraseña de administrador

Lo primero que debemos hacer es seleccionar una contraseña segura con las características que nos muestra por pantalla, ya que si no nos dejara continuar.

La casilla de **Instalar el firmware más reciente** es recomendable que este seleccionada.

### Segundo paso - Conexión a internet

Ahora debemos establecer el adaptador de red por el cual ira la conexión internet. Al estar tener varios adaptadores en red interna es posible que el Firewall no detecta cual es el puerto que tiene salida a Internet. Por eso no sale un error como el que podemos ver a continuación:

<figure><img src="../../../.gitbook/assets/image (21).png" alt=""><figcaption><p>Fallo de conexión a internet</p></figcaption></figure>

Para solucionar este problema hacemos clic en el botón de **Manual configuration** y debemos abrir el desplegable de **Choose a port to configure** y seleccionar el **Port D** ya que este es el adaptador que tiene salida a Internet en NAT. Una vez le demos a **Apply** tardará unos segundos en configurarse.

Después se debe quitar el error y aparecernos que tenemos internet como podemos ver en la siguiente imagen. Una vez hecho esto hacemos clic en **Siguiente**.

### Tercer paso - Nombre de firewall y zona horaria

En este paso debemos ponerle un nombre al Firewall e introducir nuestra Zona horaria, en este caso será Europe/Madrid. El nombre que introduzcamos servirá para identificar el Firewall en la red, este puede ser cualquier nombre. Una vez acabamos le damos a **Siguiente**.

<figure><img src="../../../.gitbook/assets/image (1) (8).png" alt=""><figcaption><p>Nombre del firewall y zona horaria</p></figcaption></figure>

### Cuarto paso - Registro de firewall

En este paso introducimos el número de serie que nos ha llegado al correo introducido al descargar Sophos. En el caso de no tener podemos empezar un Trial, pero debemos tener en cuenta que a los 30 días caducara y no podremos seguir usando el firewall. Después hacemos clic en **Siguiente**.

### Quinto paso - Crear cuenta o iniciar sesión en Sophos

En este paso es necesario tener una cuenta en Sophos, si ya disponemos de una debemos iniciar sesión, sino debemos de crear una cuenta a través del botón de Create Sophos ID. Es un proceso rápido en el cual debemos de introducir únicamente nuestro correo y una contraseña.

<figure><img src="../../../.gitbook/assets/image (3) (5).png" alt=""><figcaption><p>Iniciar sesión o creación de cuenta Sophos</p></figcaption></figure>

### Sexto paso - Comprobación de cuenta

En el caso de que la tengamos creada volvemos a la pestaña de la instalación y hacemos clic en Sign In. Después de iniciar sesión hacemos el Captcha y hacemos clic en **continuar**.

<figure><img src="../../../.gitbook/assets/image (47).png" alt=""><figcaption><p>Confirmación de número de serie</p></figcaption></figure>

Después puede ser que nos salga una ventana con un formulario de nuestra compañía, en el caso de usarse en entorno controlado pueden no ser reales.

Seguidamente tendremos que aceptar el registro del firewall y en otra ventana la sincronización de la licencia con Sophos.

### Séptimo paso - Suscripciones del firewall

Ahora nos saldrá una pantalla en la que se están confirmando los datos y verificando que todo este correctamente. En el caso de que nos ponga No suscrito, no debemos preocuparnos ya que eso significa que hemos escogido el periodo de evaluación. En cualquiera de los dos casos hacemos clic en el botón de **Siguiente**.

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption><p>Suscripciones del firewall</p></figcaption></figure>

### Octavo paso - Configurar la red

Aquí nos permite volver a **configurar las interfaces de red** existentes en el firewall. Primero aparece la interfaz A. En este caso la configuración esta correcta y no tocaremos nada y haremos clic en **Siguiente**.

<figure><img src="../../../.gitbook/assets/image (16) (3).png" alt=""><figcaption><p>Configuración del Port A</p></figcaption></figure>

### Noveno paso - Selección de protecciones

En este paso nos pide que seleccionemos ciertas opciones de protección para amenazas. De forma estándar seleccionaremos las 3 primeras opciones, ya que la última puede impedir el correcto funcionamiento de ciertas aplicaciones.

<figure><img src="../../../.gitbook/assets/image (17) (3).png" alt=""><figcaption><p>Protecciones activas</p></figcaption></figure>

### Doceavo paso - Notificaciones del firewall

Aquí debemos configurar una dirección de correo para que el Firewall pueda avisarnos de las copias de seguridad, problemas de seguridad, etc. Al estar en una red interna, si no deseamos que nos envíen correos podemos inventarnos los datos, es necesario rellenarlos para pasar al siguiente paso. Desactivaremos la casilla de "enviar la configuración cada semana".

<figure><img src="../../../.gitbook/assets/image (11) (3).png" alt=""><figcaption><p>Notificaciones del firewall</p></figcaption></figure>

### Onceavo paso - Resumen de la configuración

Como último paso nos hace un resumen general de toda la configuración realizada en el Firewall.

Es importante poder tener esta información por si mas adelante queremos consultarla, por lo tanto, es recomendable copiarla a un documento de texto y guardarla.

Una vez finalice deberá de completar ciertas configuraciones y reiniciarse automáticamente. Este proceso puede llegar a durar 5 minutos.

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption><p>Finalizando configuración</p></figcaption></figure>

### Comprobar funcionamiento del firewall

Para acceder al panel de inicio de sesión del firewall debemos acceder a la dirección IP que hemos usado al iniciar la configuración.

Ahora debemos iniciar sesión con el usuario admin y la contraseña que elegimos en el primer paso de la configuración.

<figure><img src="../../../.gitbook/assets/image (14) (2).png" alt=""><figcaption><p>Inicio de sesión del firewall</p></figcaption></figure>

Al entrar nos aparece un mensaje para crear una clave de recuperación de las copias de seguridad, de momento no será necesario crearla. Por lo tanto, haremos clic en Omitir por Ahora. No podremos desactivar este mensaje, cada vez que iniciemos sesión lo veremos.

<figure><img src="../../../.gitbook/assets/image (49) (3).png" alt=""><figcaption><p>Clave maestra</p></figcaption></figure>

Esta es la interfaz del firewall. Es una interfaz muy intuitiva y fácil de usar. Además, incluye muchas opciones de seguridad avanzada para nuestra red.

<figure><img src="../../../.gitbook/assets/image (12) (3).png" alt=""><figcaption><p>Interfaz Sophos Firewall</p></figcaption></figure>
