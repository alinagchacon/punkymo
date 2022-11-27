---
description: N-Tech Admin Group
---

# Instalando Proxmox

### Introducción&#x20;

Proxmox es un entorno de virtualización empresarial de código abierto. Un sistema operativo basado en un Debian, con un kernel modificado de RHEL (Red Hat). El sistema operativo tiene una interfaz web por donde se administra todo su contenido, las herramientas y comandos necesarios.

Proxmox tiene dos tipos de virtualización compatibles:&#x20;

* Los contenedores basados LXC (Linux containers)&#x20;
* La virtualización con KVM (Kernel virtual machine).

Proxmox es una solución de código abierto para entornos de virtualización empresarial donde domina VMware ESXi y Hyper-V que son entornos con un alto coste económico.&#x20;

### Instalación&#x20;

Veamos cómo hacer la instalación, cosa que es complicado ya que estamos hablando de un sistema operativo que virtualiza.

![Proxmox Server Solutions | LinkedIn](https://lh5.googleusercontent.com/af3UNwzL4vXqBKYG0oQdLjbwsAjTowttu0GVH-WgaSrHpwVq7BxhT0Rw02oo0uNuPkmjzNbvvyihprEYwoIMSylwWayUaxiaF4SnP2UosEEXr3WBXJTlx0vYrHdNVeYu7W\_pdhQlRPl-lGlPJ3Z-wdA3da-r7lwEpbGQRGumzuNQuQM0t80SFu2V)

![NVIDIA y VMware Una nueva asociación, un nuevo centro de datos | NVIDIA](https://lh3.googleusercontent.com/YlcB6m8USTRSSLX252CH\_KtHFmgZSRzE1cJyUuZq4K7xqdOvPeNDKSrd5JsEszTRh1co7-OmDIVQpCLnuF1lEfDWG2-mcbKpyRk3AQmtzJEKDxhH4K7Lc4Cw5LzOcmyruDxGBIuru8hGyLoVhCgrC2T-bvnaJhf-7N62f-orUvTf6Ea29GTTHYfQ)

Para la instalación, lo más recomendable es instalarla de forma física ya sea en un ordenador o en un portátil que tenga algún tiempo para darle una segunda vida. Pero antes de hacerlo se puede probar a instalar en una máquina virtual para poder ver como se instala y qué configuraciones hay que realizar.

Para ello no sirve cualquier sistema de virtualización ya que debe aceptar la virtualización anidada. En principio VirtualBox si tiene esa opción pero al hacer las pruebas da muchos fallos de instalación y por eso la mejor opción para la instalación es VMWare. Esta aplicación es muy parecida a VirtualBox pero al ser de pago tiene mejores herramientas y se configura mejor.

Pese a que VMWare es de pago hay una opción gratuita para uso no comercial que es la que usaremos. Esto es, VMWare Player.

#### VMWare&#x20;

Para hacer la prueba, utilizaremos los tres archivos a continuación:

<figure><img src="https://lh3.googleusercontent.com/FVoF2jjLQsn395c7DkjQ6WXw6t_8om5xNAOifJMUVkCRU8ty9E3YOy2giIhNWoTQx7fHMb-K-KqcCxxU6rrDpEDSTY_cA8xUm5Lha0OWkUrhdsGXBaIGZsbc_lOv1ioGNGjU6h6lnjFIBeSWWsc1hCI3UKFDLzV8XPt-BKVhZStPj-wTd5mYkQ8A" alt=""><figcaption></figcaption></figure>

Esto es:&#x20;

* El programa VMware Player para crear la máquina virtual,&#x20;
* La iso de proxmox&#x20;
* La iso de cualquier sistema operativo que queramos.&#x20;
* Plantilla Contenedor (LXC) Ubuntu 20.04

En nuestro caso usaremos Debian porque es un sistema que pesa muy poco y es muy fácil de instalar. Antes de instalar VMWare hay que tener en cuenta que si tenemos Hyper V o Subsistema de Windows está habilitado VMWare no podrá iniciar ya que no es compatible con el tipo de virtualización que usa tanto Hyper V como el subsistema de Windows. (El subsistema de Windows se usa para instalar el kernel de Linux junto a windows para poder tener la típica aplicación de terminal de ubuntu o otro sistema Linux.)&#x20;

#### Paso 1:&#x20;

Instalamos VMware Player y una vez hecho nos saldrá una ventana como la que se muestra a continuación:

<figure><img src="https://lh4.googleusercontent.com/9693CEuLlkQC87F5nkKmvfWjLsoDsdzVYLSrnBrAbU30AwYOedo-pzave1ZHC7wB8KzgweZ9R2YaYYQuf0Z4GIFyioTAAQ1jQ7ShWl1RBg-mELgLEDrv-piHhHPlIq0zHAfutq3xIX9fpat24UJkYI7QaWW8ywaNzGtuM63URVYD0aHtj22IEbxu" alt=""><figcaption></figcaption></figure>

Creamos una máquina nueva como muestra la flecha roja “Create a New Virtual Machine” y se mostrará la siguiente ventana:

<figure><img src="https://lh4.googleusercontent.com/gn8ArZPlaJ9I8DjJVpMp1FXiFfJGsDZAkRzJPhwHQMZq-2QjcKOUzD-nqs3rXZ31Pom1WdNKsWrI5t2LEb5A9T72JZQFb1Er57crBLHzJqM_THiacgjVsripNmEiXU9iYtXUuepzTLI2UkPetq_JVdkSZ38pVJ01IZE_KIHmZdwRBEtCO9CpeiCq" alt=""><figcaption></figcaption></figure>

Como se puede ver en la ventana se pide el método de instalación y como se ha mostrado antes usaremos la iso de PROXMOX. La añadimos desde donde la tengamos almacenada y le damos a “Next”.

<figure><img src="https://lh4.googleusercontent.com/3zOTTzgEf20OTbG3Zh9XGkmzt2ZFL2mKoZgUHJVABIbgg0YkCrW6yMTgxFffcUKkpTOkdYyhaUly7WFiXOi2Fn5OUpBVYmFslGQCmf64v6TwI9KEm01RQrAMHsBwTbeTxngot2I8XaPidaBFm8j02iwryqH1E6U2-RKq6Fcg5O03S7Q7cdFxGj7Q" alt=""><figcaption></figcaption></figure>

Ahora nos preguntará en qué distribución está basada la iso que le hemos añadido, como hemos comentado al principio Proxmox es Linux basado en Debian y Red Hat. Entonces al poner la versión de Linux lo más recomendable es usar una distribución modificada de Debian como por ejemplo Ubuntu o simplemente Debian.

<figure><img src="https://lh3.googleusercontent.com/wNCR9qTfw_xEXcSssri_Fdh1982J0nsBfIvQK2zKvXgjTNLAMUunF2kZ_5PFuFID7hnd0CWoSwCBNEMdRx2KpKOQuxctyVpiYm_aGZ2y0fyGXp8Uit8qwsSwSt_BeuNeEtKZNW1Do7h4VPINY9Q2P7vdFMzE8lQPkYLTOm0iZIXML102ICLGRdyI" alt=""><figcaption></figcaption></figure>

Llegados a este punto nos pedirá especificar el tamaño del disco que queremos: lo recomendable para PROXMOX es 30 GB ya que además del sistema operativo hay que almacenar las ISO, las propias máquinas virtuales y los contenedores con sus plantillas. Por tanto se recomienda proporcionar espacio suficiente. Sin embargo, como es solo para una prueba con 30 GB es suficiente.

<figure><img src="https://lh6.googleusercontent.com/NNWc3laDp-JBccSL7jhE5zivz9aLAo8z9A_i2Aeq5lngFQpY1MfnWSvFfBuhzB4APAHlknu8XYGKKfXdUYQJfpgrZHuFjuGU3L3VC2Fp49ZOoqJU9NOPMz0A9LxL3Po5VB8tA9-qGsqfqgxH-NChmJkCXj6NTxhQEf3cZyHV-NtbkzjlsobjQdZt" alt=""><figcaption></figcaption></figure>

Ahora se nos muestra el resto de configuraciones que ha creado por defecto VMware pero hay que cambiarlas por ese motivo antes de darle a finalizar iremos al botón que se muestra en rojo en la imagen “Customize Hardware”.

<figure><img src="https://lh3.googleusercontent.com/oWGRaF_wR8_9pKTe1Ir0Ls5P0SH7wYPfQT6ZTExWIKSYLWGi9QxcvXe2se2Rj3UHEP1yiwsgpUn1i9kbTj04HPak4xO81a3SQsGvyuSsC6D0wMmKwzubKQKi0QIMZB7H45KH74gQOrAH_6fWIPKUunL1LWxoOn6aBZy24r2roWI1RXB0_X47QlPv" alt=""><figcaption></figcaption></figure>

<mark style="color:red;">Al crear la VM me dió error y tuve que desmarcar la opción de</mark> <mark style="color:red;"></mark><mark style="color:red;">**Virtualize CPU performance counters**</mark>.

<figure><img src="../../../.gitbook/assets/image (12) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Aquí tendremos que cambiar la cantidad de RAM que se le proporcionará a la máquina: lo recomendable como mínimo es entre 4 y 6 GB. Una vez ya seleccionada la RAM vamos al apartado más importante: al del procesador como se muestra en la imagen anterior. Hay que tener en cuenta que el procesador del que disponemos para dar más o menos “cores” si tenemos un procesador con 4 núcleos y 8 hilos le podemos dar entre 3 y 4 cores. Pero si tenemos un procesador mejor con 8 núcleos y 16 hilos le podemos dar 8 cores.&#x20;

Cuando ya tenemos seleccionado la cantidad de cores hay que activar las tres opciones que se muestran en pantalla para activar la virtualización porque de lo contrario no podremos instalar el sistema operativo.

<figure><img src="https://lh4.googleusercontent.com/EyT48T0yI9Gog5brE2h8Mxbs6OkFr1dppTKEPNXQtCiiTR_lRUw7kg_Tk57rCOtnEW2qgUhLdnKqoNdyBN3E89rDKuJ_JUJ-z18Zip2P8xZ4XckwMGCdWPUhAYExQl13n_HXN3PPjSC509Fb_Kw9L_s_7UPGEDocEQJR8eCi9fXB4df0GNcIZWLr" alt=""><figcaption></figcaption></figure>

Con esto ya tendríamos toda la máquina configurada y ahora nos tocará iniciarla e instalar el sistema operativo. Para hacer la prueba el adaptador de red lo mejor es ponerlo en red interna.

Iniciamos la máquina dando al botón que se muestra en la pantalla.&#x20;

#### Instalación&#x20;

Lo primero que aparece al iniciar la máquina es la pantalla donde podemos escoger si instalar PROXMOX y hacemos un test memory.

Lo que vamos a seleccionar es es la primera opción “Install Proxmox VE”

<figure><img src="https://lh4.googleusercontent.com/iKOZ0S6YtLXse_vWhkwNjqwmIPb1P5dFkJSwiZVsRiR2hLj5RA2fY6JW_JxWVyHllZU3mSt7r1nSdm12LVM1ZMjMkdTngyaynU0jJi1XBhPyOi0YoMx2FrZV-p88llnOQTgx2mZZ9YLYwJ7xmSOffhzW558x3-9JaudCdBoTBoZZF4Js_XuUquSG" alt=""><figcaption></figcaption></figure>

Una vez aquí hay que seleccionar el disco duro donde queremos instalar el sistema operativo. Como solo tenemos uno creado no hay que cambiar nada.

<figure><img src="https://lh6.googleusercontent.com/8cKQslSS37aLO35G18FKvrAXQZrNhFg7C04f2rDO3LcT-Ax68OhbU1hDGO3ZYQZarl2HXcliYaOYFKsNTapeew5bdVUd6eVOl21shEWQHMvUVzHwX3Tey5qsf0sLld8V6V5X279-zfe8sfwppqWUBTLC_tydZnuYDfzYWtvIM3vCAm1QVu7bEYSW" alt=""><figcaption></figcaption></figure>

Aquí tenemos que configurar la región, la zona horaria y la configuración del teclado.

<figure><img src="https://lh6.googleusercontent.com/1SG2jGslF3lwM5i6x_j-LrJbEOJiB6M1sDpscUHtCWyIcSPobTxdEkcXwgT0_VbgFn_BBn9cc_Klduv_dukdSJCD2oR-U9pgHJ50jq52WTcbz-O0k9cL5NtMsXNSTg7fXAbE32bVUM6EmvvuW6Y9vbkqlBcHQ2huSM9jtqw_gvl7A0Ww-5XqrSbd" alt=""><figcaption></figcaption></figure>

Una vez hecho nos pedirá la contraseña que queremos poner para el sistema operativo. También nos pedirá un correo electrónico para enviarnos notificaciones en caso de que haya un fallo en el sistema. Para hacer la prueba podemos dejar el que viene pero hay que cambiar el “.invalid” por el “.com”.

<figure><img src="https://lh4.googleusercontent.com/KKgtAg8IwYaynWlkcTqOEsyvyPwDHR16_6qDXog3KWmZUF6Ncc5VxlZD7nkikRZIgQ_qgLkXHqglNFZ3bkSNDUJy_Ageyp6_v1OOdl3IqWyxkO0_rEkqQfcpg9GTH-RjgSExBhqPsAx2zDc2CfhPVG1UMr-u0Y71DevDF_o0qgj9gb9WloV77NEr" alt=""><figcaption></figcaption></figure>

### Configuración de red&#x20;

Ahora entramos en la configuración de red que es el apartado más importante de la instalación. En este caso, si ponemos mal el rango de red o la puerta de enlace la interfaz web no funcionará o el sistema no tendrá acceso a internet. También hay que tener en cuenta que si hay más de un adaptador de red hay que escoger el que tenga acceso a internet porque sino no funcionará. Si estamos instalando en nuestra red basta con usar un rango que, con el DHCP, será suficiente. Pero como en nuestro caso la prueba la estamos realizando en el centro hemos tenido que hacer un escaneo de la red para poder ver qué rangos no estaban en uso y configurar la puerta de enlace en el rango de DHCP para poder tener internet.

Lo que se muestra en la imagen es un ejemplo del rango que puede que no funcione en el caso que ya esté en uso.

Una vez configurada la red hay que poner un nombre de dominio. En caso de que hubiera un servidor con dominio local o tenemos un dominio se puede poner. Para hacer una prueba se puede dejar como está pero sí que es recomendable cambiar el nombre del servidor por si después lo queremos unir a un cluster. En el ejemplo de la pantalla el dominio es “pve.localdomain” lo que significa que el dominio es local y que “pve” será el nombre de nuestro servidor.

<figure><img src="https://lh5.googleusercontent.com/_s2utKMavB4WN7YA-f7YtwE-fSbUNr6jxV7p-RtqbO7gl8ht-6ZVJJZQMOHBKL0pd8uY4pdeHBEZ3b1KCtOi2n4-Er8zclKM59syfRfhjAhS1cIo43-eI1yx-2vnSiDyEB47_RGn6SNNPXKnVTJ3ofiQUvhsufxN7lV-9nwpN6hH5Bw6YklIzRVw" alt=""><figcaption></figcaption></figure>

Cuando ya tengamos la red nos mostrará la siguiente pantalla en la cual no muestra el sumario de las opciones seleccionadas durante la instalación para poder comprobar que todo está bien antes de instalar el sistema operativo.

<figure><img src="https://lh5.googleusercontent.com/pAk4YdfRgIjXSdLEnC9kchaA_n82LVhu64S9OGrGTlmzHnK4MNGEtftjn9zJulheLlljHIPVmb1s7lGOtLnwqriyK7U6ldsUZ5S-DKXXKbk8MT5m83vOXTBWWwDevoXxsWDGOSGFrTcoMVP05jcprRRrYvmdykozjKDlh8f_ultg4YoFtGoeojh1" alt=""><figcaption></figcaption></figure>

Con esto la instalación ya ha terminado, el sistema se iniciará y nos mostrará la siguiente ventana:

<figure><img src="https://lh4.googleusercontent.com/oFnnAFFfR33CJNf8L_R2FiLvc4vbOIAHagI1oEbZUL4LZ3MO70N7qLOkMk6eSqH-Linx9PXQGdLpb74wyKyWPdHhQRb1xwdm_enj6Wni4YGTakttC1sYsjlAIw_oaD5vxs4H9FM0Vl8sH0tHfxmT8QmGk68R8GPz-GhhYNuXCmA5UnJmaN9pwllo" alt=""><figcaption></figcaption></figure>

En la imagen nos da una IP que en mi caso es 192.168.100.2 y con el puerto 8006. En cada caso será una IP distinta pero siempre será el puerto 8006. Ahora tenemos que entrar en la interfaz web que es donde se administra todo el sistema operativo, por eso nos hace falta la ip que tendremos que poner en el navegador de la siguiente forma https://XXX.XXX.XXX:8006 una vez puesta la ip en el navegador nos saldrá la interfaz web de proxmox la cual nos pedirá registrarnos.

El usuario administrador por defecto es “root” y la contraseña es la que hemos especificado al principio en la instalación. También se puede escoger el idioma en el cual estará la propia interfaz web.

<figure><img src="https://lh5.googleusercontent.com/mK55yTpA9fPMThWpkjhdXtAmvgznkRlIUxrN5YgXgOqSi0wXmVgvIMVCUbXp59ZZn94PChBOxywChQKbrdEcBP0zY8Mxxe5k-vXv_6JHEwV8SB_AOwwwwizy9YAJHD5_1O-uoXzfz5VNf6bfKYKzXEfNCr4Uip-1EhV_wsxwaDAL1ZEqu5wILYlD" alt=""><figcaption></figcaption></figure>

### Interfaz web&#x20;

Cuando ya hemos iniciado sesión nos mostrará la pantalla principal de la interfaz donde se pueden configurar los aspectos generales del sistema y poder ver el estado del hardware y su uso, configurar permisos de usuarios y grupos, configurar las copias de seguridad o incluso añadir nuestro sistema a un cluster. Como se muestra en la pantalla lo primero que se ve es el nodo (nuestro servidor) que se llama “pve” y que tiene dos particiones llamadas local (pve) y local-lvm (pve).

<figure><img src="https://lh3.googleusercontent.com/18MFxgYdaa-GO6Gr8pJ2xG540PZTVBIQYJ-E4bPNNMSdxEyz4WFSbvi9b1bhFugGYWxEzcDx11_-mHEf7rw7M0j7gYCk31O7cP5xm3V5yOKLOOMltdvXDHAF08mRJ5qaXvJstJtvUvFvcxDhuMyp4ISgaWj59oXgABjhn3y9RohWLGTh7gV6Wnw2" alt=""><figcaption></figcaption></figure>

En el primer apartado de la configuración central llamada “resumen” se muestran unas gráficas del consumo de nuestro servidor así como las especificaciones técnicas del hardware.

<figure><img src="https://lh5.googleusercontent.com/-5oKkHrBvWdq7e2CFTy2I4Y0I-vGtmsylqYlxhTsX5GK6YWQJcQdLWNtaz84ajLV6Etw4l_utiX1KFLpUGN_w0eXU6Lh1TEm5YuWQKZDTwG6W3SGi-3So74l6TstEIlwsDpe8TPrSgr330yYkTh4osl8kddMtvhQfS3a4_kDykgsDHqzSW4IEL3B" alt=""><figcaption></figcaption></figure>

Ahora nos dirigimos al lado izquierdo de la pantalla donde se nos muestra en nombre de nuestro servidor, es un desplegable donde aparecen todos nuestros discos, particiones del sistema y cuando tengamos creados VM y LXC también estarán listadas en este desplegable. Pero para poder crear las VM y LXC nos hacen falta las iso de los sistemas operativos y las plantillas de los contenedores por ese motivo nos dirigimos a la partición “local-lvm” que es donde se almacenarán las iso y las plantillas.

<figure><img src="https://lh4.googleusercontent.com/z4mxtlDynd10o8zL0zKROUx3IFPl-YQLAm-qGpnurfoku6yEv9RCkJRVr8JtQAsJ178UxJ7sQhNZS7GuhzQeJnQi9AC1TKRnZ3zpydty1AJklDzhYdk7QdXBGwSGjqeZ36-Xic2OWs3aGif_n9noXvhbJdJkF4uQeZgbRGScBfH8jeGjBGN0T9Av" alt=""><figcaption></figcaption></figure>

Una vez allí de forma continua tenemos un gráfico del uso que está teniendo nuestra partición. Justo debajo de la opción “resumen” tenemos las opciones “ISO Images” y “CT Templates” que es donde se pueden subir las iso y descargar las plantillas de LXC de la plataforma de proxmox o subir una que tengas tu.

Para subir una iso a PROXMOX nos dirigimos a “ISO Images”, que es donde se mostrará el listado de las iso que tiene el servidor almacenado, para subirlo le damos a “Cargar” y nos mostrará la siguiente imagen.

<figure><img src="https://lh3.googleusercontent.com/c-SXpsHD5WVehFvSaDqsm2s8EIzgRcY1dS4lU4lDGtWzmZU43xp8NeZb_9b2XuUN7-Cv4K5aQi6LzbnSCkvPU61E2Ly11seFD6stQUzslk2BEZ6q4NP2ye6aZviuwSjlM5cO4_NaALJBBizqt2M-Lja95X2zlhnTEt6D8ihzrTTG0UY04AD7IH_O" alt=""><figcaption></figcaption></figure>

Como se puede ver en la imagen anterior nos salta una pestaña donde nos pregunta qué contenido vamos a subir y donde está almacenado el fichero que queremos subir desde el dispositivo donde hemos iniciado la interfaz web. Se puede cargar cualquier iso siempre y cuando esté en el dispositivo en el que tenemos iniciada la interfaz web.

<figure><img src="https://lh4.googleusercontent.com/z4mxtlDynd10o8zL0zKROUx3IFPl-YQLAm-qGpnurfoku6yEv9RCkJRVr8JtQAsJ178UxJ7sQhNZS7GuhzQeJnQi9AC1TKRnZ3zpydty1AJklDzhYdk7QdXBGwSGjqeZ36-Xic2OWs3aGif_n9noXvhbJdJkF4uQeZgbRGScBfH8jeGjBGN0T9Av" alt=""><figcaption></figcaption></figure>

Para descargar plantillas de contenedores LXC o subir las que ya tengas descargadas o plantillas creadas por uno mismo, le podemos dar a “Plantillas” para descargar desde repositorio de PROXMOX.

<figure><img src="https://lh3.googleusercontent.com/npcR3hu4W-_MrmPTRRJ0Z4WFRKaz-FZRlGMaeixoDN4atpw6yMxOfMupUnkzzUSBPMuApQmDFZOgKfofBq2zXA_rlratM8Fc-NzQEiXC7_r8g7DyprGQiYwd1QB4ixWwoQa2aKDfMHpOxQP4t_HIrvakzkX94SRvskKf9XOKGE6lCVNR0K8CVsPX" alt=""><figcaption></figcaption></figure>

Cuando entramos en “Plantillas” nos sale la lista de plantillas que están en el repositorio de PROXMOX.

<figure><img src="https://lh6.googleusercontent.com/TTvz9ZDl7W3lqeLtmBtrPpVxnziMkDDFxD4DH0Ye2yUwm3VEEt_oHVdyeewz7pxLltP913ySc4EiTVLZr9jJTHMnIyHU_mzvnhCLnwGjViZn-djcgNMjHlkfEHoDGd--5Y_Rn0rHWpwfvTMjnzfrckDeirWZc3l0aBou-tclMZG-q9nb2xAn7Dbm" alt=""><figcaption></figcaption></figure>

Si al descargar cualquier plantilla nos diera error podemos descargar la plantilla manualmente en la página http://download.proxmox.com/images/system/&#x20;

<figure><img src="https://lh3.googleusercontent.com/h6gaZ5VypTE4L2h1FxdUiJJ8WVAYhactCn7g2udnipLn46xClX-I8qnu0piexCv68dKqUwf3BR1DP6IWqNK5sua3Zn-jTXIoY9mz5pov7Mqj7JdsEs5cgtQkhLgGJa43GnElBYOvj7OR6_WNgsunFBgFO-oo-xsuAugfNGdv2yWrcJAjQPsymfEM" alt=""><figcaption></figcaption></figure>

\
Cuando la descargamos o si quisiéramos subir una plantilla que tengamos, la podemos subir de la misma forma que hemos subido la iso en el apartado “CT Templates” y poner “Cargar”, nos saldrá una ventana muy parecida a la de subir la iso pero para poder subir la plantilla de contenedor como se muestra en la imagen siguiente.

<figure><img src="https://lh4.googleusercontent.com/xzWdwCpfJsziZ4ssxqCCUXA-mgNiWNAJTDm0viysGO91i_A2ZrAzMG2YIKfcIc6XOog4AzNuTDnEagmB7kci6sFbkJkLV8poiZkjOqkuwyVJkf52BgWPBrvxmhEDIEs7zB1zEjoMnIeDo5yy7_kwIV4Ui4qPAFUe7Sq436-MVj7HjSvPJZL82nLd" alt=""><figcaption></figcaption></figure>

Cuando tenemos descargada la plantilla del repositorio o subimos alguna de nuestro ordenador nos saldrá el listado en este apartado como se muestra en la imagen.

<figure><img src="https://lh3.googleusercontent.com/npcR3hu4W-_MrmPTRRJ0Z4WFRKaz-FZRlGMaeixoDN4atpw6yMxOfMupUnkzzUSBPMuApQmDFZOgKfofBq2zXA_rlratM8Fc-NzQEiXC7_r8g7DyprGQiYwd1QB4ixWwoQa2aKDfMHpOxQP4t_HIrvakzkX94SRvskKf9XOKGE6lCVNR0K8CVsPX" alt=""><figcaption></figcaption></figure>

Con las iso y contenedores ya en el servidor podemos empezar con la instalación de de una máquina virtual. En la parte superior a la derecha tenemos un botón llamado “Crear VM” como se muestra en la siguiente imagen.

<figure><img src="https://lh4.googleusercontent.com/sJZ-sY1DmPchbjpFS8Aml48UgzUCiTYq0Fio9YleC-8_bPJsHTWX_YVK4ho6JK7GZBFDDkzp5KSKkNfdEDcvlmpksma8B0QuY-bDV6hDSZktMhiJfAAGFGDeClxQRijEUQK0UEojZbBPeyvkRuej-JuSuIo_uY2-OHZ3Am5mJ3Yg9P0c4wzrMiOu" alt=""><figcaption></figcaption></figure>

Nos saltará una ventana para la configuración del hardware de la VM. El primer apartado son los ajustes generales como en qué servidor se creará la máquina (como en nuestro caso solo tenemos un servidor y no tenemos un cluster no dejará escoger el nodo donde instalar). El VM ID es el número que le asigna el servidor a la máquina al ser la primera que creamos la ID es 100. Y por último el nombre que le queremos poner a la máquina.

<figure><img src="https://lh4.googleusercontent.com/Ela4ZMsX-1s8MSYP9EvJHW1s3qqaKYB-3ZPYVWHc-ffz7OTI4foOZC9mwSC7Zj09_GlXbetEafMXWMkQIP_Fyczn-TCwwOQiPZTXbCmdHFXgiHfgqsCfUV1XRbmcNoB5bCPbi1RASZ9QT12iTqF0_yVydjccLvURNxEq1bDFDtjFctnx975BvIuC" alt=""><figcaption></figcaption></figure>

Ahora en el apartado “SO” tenemos que especificar de qué forma en la que queremos instalar el sistema operativo en la MV, podemos añadir la iso que tenemos almacenada en proxmox, o conectar físicamente un disco o usb ya booteado al servidor o no añadir iso. Cuando hemos puesto la iso tenemos que determinar al sistema si es Windows, Linux, Solaris Kernel u otros y su versión (En este caso al ser la iso Debian es tipo de sistema es Linux y versión 5.x-2.6 Kernel).

<figure><img src="https://lh3.googleusercontent.com/kyQ-xF5Cteg27fMhIpI067p-mCjXgLnqZc0ivjkqMQUr-f7Jc4C3FDPTvGb_pLRGB9ZhBbXc0nEepz6BZFEfzRg-7PUJ0nV0A0Benxs6O3qfeB-FS3eMrdpTyRQ1vY7ga5j5PzkxQibmo8QLQmZi4hgFj5c6V1YIj8wRU5KUWFzWIPyyddC89QSX" alt=""><figcaption></figcaption></figure>

Una vez seleccionado la iso vamos al apartado “Sistema” donde nos preguntara qué tarjeta gráfica usaremos y el controlador SCSI. En esta parte lo mejor es dejarlo por defecto.

<figure><img src="../../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

Ahora configurar el tamaño de disco, donde se almacenará la máquina y el bus del dispositivo. Para el tamaño del disco con 8 GB para debian es más que suficiente. Si fuera un sistema windows por lo menos sería necesario 32Gb de almacenamiento. En cuanto al bus del dispositivo y la caché se recomienda dejarlo como esta.

<figure><img src="https://lh5.googleusercontent.com/fgEzFv1o94_q_nz3gXzSsdUoB4-LZJW4fGrlVU6RBud1ZNv29JCwzEHwlPKRM9VmGzzvrU2zzJYk4jxJfgLjUaf7zzKkZLL5d76yN0RVS7NLUHSujdBnJzrcmsLETLIvljNDJ7ICF9bGTeHzhdL7O5UiDyoYjsOQu0UXWm1dN0N0fdlHJeBLnv6q" alt=""><figcaption></figcaption></figure>

Nos dirigimos al apartado de la CPU donde tendremos que especificar la cantidad de núcleos. Para una VM con dos núcleos es suficiente aunque si disponemos de un procesador con muchos núcleos es recomendable que para sistemas windows tener 4 núcleos como mínimo.&#x20;

<figure><img src="../../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

Una vez definido la cantidad de núcleos que tendrá la máquina, hay que poner la cantidad de RAM (En el caso de un Linux en este caso Debian) 2GB de ram es suficiente aunque se puede poner más. Es recomendable que en sistemas windows el mínimo sea 4GB

<figure><img src="../../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

En el apartado de red hay que configurar qué adaptador usará la máquina (red NAT, adaptador puente o red interna).

<figure><img src="../../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

Una vez con esto llegaremos al último apartado de la configuración de la MV en la cual sale un resumen de todos los ajustes anteriores para comprobar si son correctos antes de crear la máquina.

<figure><img src="https://lh4.googleusercontent.com/NCFj2Ytg67wA8DxM2ivQarC5q7J31Z3w5o3RoDtAbu8gOaB4wO2W88aFJYt1JVmHM60qhN_ATY_Y7dhXKMxVhddYd8E66ra1D1J8i4ZxpZUTUe1hX2AiXMV9FkI82ic0r1XQv4aIw1Bajq8ntPGWIzbyLwSk70fwu5G34W3hE_I1ky6GysDTsz6b" alt=""><figcaption></figcaption></figure>

Cuando ya está creada, como podemos ver en la imagen siguiente, la máquina aparecerá en la lista de dispositivos en nuestro servidor (pve) donde aparecerán todas las máquinas y contenedores que se han creado en el servidor. Para iniciar la máquina nos dirigimos a dicha MV/LXC y como se muestra en pantalla le damos al botón de iniciar.

![](https://lh6.googleusercontent.com/BaEHBWVGxsY8yQzoPdX-vHHBzR0m59pdT3ugJAnK18VPQisXsL34sSf4jCilqHzj-KXbLFrSRtZs5MX6DyxP\_DNsQakhqZpzptpoqU18FRrz6m6N8bMfX9eFdLbeZRpCHuYxB58UVo4Frz8ni-MYfy9V39Fu6YLEQ77mHDKkdkzDg3CNENnH0ryg)

Cuando tenemos iniciada la máquina virtual, tendremos que acceder a ella, hay muchas formas de acceder a la máquina virtual ahora haremos de la forma más sencilla (por VNC) que simplemente hay que ir como se muestra en la siguiente imagen a consola.

Esta nos abrirá una ventana donde podemos ver la maquina virtual.

Ahora solo habría que instalar el sistema operativo de la máquina virtual y ya lo tendríamos listo.

<figure><img src="https://lh3.googleusercontent.com/tTG_p9GI81xT8ID_PF22xEyC7LW5aF1wm8euLbSgpSSJhvaQxz6B23vfBIIZxQTfJnkqDDg8qTaMdKPRGHMH2sLqJk71F5fIBU1A8IfH5Lm0D2JMLHSd_GAQvIO0BpH4m6Awiu7AK-DKqXmRcbKcTq5KglvzKMh5_xiXBW13E0oJsHyQlY1unaCb" alt=""><figcaption></figcaption></figure>

De la misma forma que hemos creado esta VM con debian, se puede crear con cualquier sistema operativo. También hay que tener en cuenta que si disponemos de un servidor con dos gráficas y queremos que un VM disponga de la totalidad de la gráfica ya sea por que queremos tener una máquina virtual para jugar o tener un servidor de streaming, siempre se puede configurar en el apartado de “Sistema”. De la misma forma si queremos utilizar el protocolo “SPICE” como mostraremos más adelante habría que cambiar la gráfica por defecto y poner una compatible con “SPICE”. Por último hay que tener en cuenta que el formato en el que se crea el disco virtual por defecto es “RAW” que no es el formato al que estamos acostumbrados cuando hablamos de VM (VHD), pero siempre se puede transformar en ”vdi” (formato de VirtualBox), “vmdk” (formato de VMWare) entre otros.

Ahora vamos a ver como crear un contenedor LXC, de la misma forma que la VM tiene un botón para crear, LXC también lo tiene. Está justo al lado de “Crea VM” y se llama “Crear CT” como se muestra en la siguiente imagen.

<figure><img src="https://lh4.googleusercontent.com/yjqFWhC3YeZ-YB_GnMW9eggZZ9BN753aEjJ8RATJrY_wieEZ-Uib4CBc1PBRLbguvhr_QosXJdKjmzw_gpBGR4RFNGmX7wrzlEDLHhD6xFMTy_xnYfX2EPf9X1lbV2vs2yUQuisvmQVgWfTPG9Zm5ufHCDQ92LmrwJRb6s7i2Vp-X0mgDrEXnf-w" alt=""><figcaption></figcaption></figure>

Como se muestra en la imagen siguiente la interfaz para crear el contenedor, es muy parecida a cuando hemos creado la VM. Se nos pide especificar el nodo donde se creará la máquina (como solo tenemos un nodo se deja la opción predeterminada). Después nos pregunta por la ID que tendrá la “CT” como es la primera que creamos es la 101, seguido de esto nos pregunta el nombre que tendrá el contenedor. Y por último la contraseña de la máquina, y si queremos que tenga clave pública para el ssh.

<figure><img src="https://lh5.googleusercontent.com/tgwsWTOXHBuPqrKNsg_Z5e9GcWzwf2EYlYumDCGjuSHoeoBxes9IfupMazubW3K9is0jDwUbRdu1K2R7NZCdoJ3xVSvuey59xYTLWAqqsSOVPk67bedsQpY4c5f5D9DjBD48_oOczzA_oxc5fgXGIAssc63jB_l8c9d0yW0lVX9sUDjjqgGNTH2L" alt=""><figcaption></figcaption></figure>

Ahora toca seleccionar el lugar donde está almacenada el contenedor (plantilla) y cual se usará (es como la iso de una VM).

<figure><img src="https://lh4.googleusercontent.com/hEgaDOoQvP8FmsX2Yh0K5-AWTmxIMY8cgtNvQQA_riIrx14qJg86M63sc1muqHE_3Ltl_xpTXzduQZcf7EDLY6Q3P48SieBUtRUh34bjKhl-Lphc_20FQWdczZMNJVrzhSIdc8deJICxNgQLBHTl_nU4OX2igXkR8k5R-TmNfQR-s_PedCurKCmz" alt=""><figcaption></figcaption></figure>

Una vez seleccionada la plantilla hay que configurar el tamaño del disco virtual. (Para Ubuntu lo mínimo es 8Gb).

<figure><img src="https://lh4.googleusercontent.com/Q86zW_7PMh1YRhtOsS2Fc7iMf7yq660csdpRlObYiMYwiGAKFv5LjDPaucnracRUIcop-fSKtN9cisq5QBgpztAOAMGALNa22_coL5KZylulypSURFiydpU1K17az7bCpxP7lPMVpkQP7gRcSz75Y_Sz0_Tkc4-Jc_ikQkzBCYuxSTQEjDEF8vBt" alt=""><figcaption></figcaption></figure>

Ahora hay que seleccionar la cantidad de núcleos que tendrá el contenedor. Hay que tener en cuenta que al no tener un sistema operativo con interfaz gráfica y que el propio sistema tiene recursos reducidos para su eficiencia, la cantidad de núcleos irá dependiendo del servicio o servicios que queramos instalar en el contenedor. Si es solo un servicio básico con un core es más que suficiente.

<figure><img src="https://lh6.googleusercontent.com/ZoSfXzDqkLWOEE5KUE3FjGg0xHb1YsecX3qfPuW7gw8IVApZqFUOf3qHjjNdgEtXZvLpAIer0QTsHblI31srMKp2sGT1YX3YefzBIagyCgoV08S3zdOpHwSyzSP_9Q0Tylp-Wpsv0BLzG5LQNb18rWoUHbPwP3jmkgrXMZFIsyg9bny9ZThwruE-" alt=""><figcaption></figcaption></figure>

Como en el apartado anterior, ahora hay que seleccionar la cantidad de ram. Que hay que saber como en la anterior qué tipo de servicio instalaremos, para saber si necesitamos 1Gb o 8Gb… Para hacer una prueba de instalar un servicio básico con 512Mb tenemos suficiente.

<figure><img src="https://lh6.googleusercontent.com/EskmduLKzq8DApUcuNJLZRtKVhXnisvVbBCCAgeUiw3QtbVDk2NNNeXgoFMMsIub_QBqeEZYYiz6niUdMFYrEthKj-ENH6pLrTyyo0txx-EJj7SDrlFL8v-XOK28vHai-cb9grggrU8b7WtuhpKZqn-aCxPzER92Yn_tJLid-_nTFdCMAf8BhcTB" alt=""><figcaption></figcaption></figure>

Ahora vamos al apartado más complejo de la instalación del contenedor, hay que seleccionar el adaptador de red, si queremos MAC automática o ponerla manualmente, si está en una VLAN (por si queremos que esté en la DMZ ), si va a tener ip estática o por DHCP.

<figure><img src="https://lh3.googleusercontent.com/sWzTBLvjefND_efcOG6HId8IItR2PTwft78bkAH-8VgfjMTh-dautwb_3GqM6SujKybyz-w65YO2gFXeZIUePDJawIuppM5UEKU7o_FRvUV6ycq7ec9x7gxnqmFamnNTOFSJmqLSdY3QyCMyOgYBJHZWVYXkKAaYpxktNrBuoLNBed99quPk7Hn1" alt=""><figcaption></figcaption></figure>

En este apartado está destinado para añadir un servidor DNS en el caso que queramos usar un servicio que necesite un DNS propio para el uso. En caso de que no lo necesitemos, si lo dejamos vacío usará el DNS que el sistema tenga configurado.

<figure><img src="https://lh5.googleusercontent.com/0RCzeqCQtMl1ySfPJYP6QqbTcB7aKtwIjGnjsRaOwb5WEw79zhXzl1mP2DcAoPDnQ0V0dgZVUk0VuBtpguQ75aYwUJjQME0RzDIsqGyCWP0LqQR_UMLGxxxSXsPqkjcUKUYEFlyasSNC59PEol8rGSXxYiYxt90E77JVlTN8Z3wttEeoY13JiYc9" alt=""><figcaption></figcaption></figure>

Y por último nos sale el listado de todas las configuraciones que hemos seleccionado.

<figure><img src="https://lh6.googleusercontent.com/zhld3-NrnbhkNEY8OVgB_IXo5zVKof3xgkp2HAL5YGDy-mZhQpnB1tqpcQV916c6vQAUdFcClXYdDa-ahXZxnYZjIQr3jy0BLVYAEn3MD99RVshW6M1SsObx4qsiVZS4qvKNa3sISeXi9RAhlN3-H9sGyisZJiBsd7fN5TODsurl6NNEXBVFk4VS" alt=""><figcaption></figcaption></figure>

Como podemos ver ahora está construyendo toda la base de configuración que hemos seleccionado antes y cuando esté listo tendremos el contenedor creado.

<figure><img src="https://lh6.googleusercontent.com/bK9nFa545BYNLpiYgwSDq_52XJFeyVHaMWigqhSb50SXPsd_0mI0265oKBYQ_WBwz_vtc4t40l_ZsU7GBBFfXByyUQXj_J6kki_nQk5zeWsqBC1SHFjuc7u202wy9ixT6H6RllWSV4cfg4t2b6ZMGGBppanVAqZjmJw-TwwKQVBlVTVEB6P7jMS5" alt=""><figcaption></figcaption></figure>

Como anteriormente hemos creado la VM ahora la LXC también sale listado el contenedor, como se ve en la imagen siguiente. Si seleccionamos el contenedor le damos a iniciar.

<figure><img src="https://lh4.googleusercontent.com/jbODrZhPBD3h4nCs1FiAyj1FR0lZgjgNaSseWVZmz7UL_R5EYdcsgQRqj_Z49K4RGBqGmkXtGqqm0kIu_bRPr-CdYkGLbc6uRbEcWnCLQ8wcpfQDZqUCvJjPtroBCvWxEt5LLCmWfbTiPHhoV2JpQQ2KlH-pGiLlkf6SW8vyVCTjJbqqdE2d7b6h" alt=""><figcaption></figcaption></figure>

Cuando ya lo tenemos iniciado seleccionamos la consola.

<figure><img src="https://lh5.googleusercontent.com/g1JtrCEusMUtuD-LPCxw98vekl9jmy1BANRzM4xXnvpgHjp5h6-3InLb2Roq2eV5ZnINXS3Xp_i01dFcrEd8iGijZJRdlPM0EYu6QkDG-ys-W5-4W5ndX_5_zt8_ad1RtaJPnxcVPn0NUGhZQTAwnzB_FNk0xd3sB-uRQk7CPvb3O61gjhM87zck" alt=""><figcaption></figcaption></figure>

Nos saldrá una ventana emergente como cuando lo hacemos con la VM, y ya la podemos usar de forma completa.

<figure><img src="https://lh5.googleusercontent.com/n56pf-EKXvJDSCRN7UHUJMvvROE9KZlORFCFTScezv7HKH_PYdsy6_yCT7IkFPLY5WkXI7_5xKOqdhkgRivToZjZN36W8lPq7GkBZY-Mf2-_ktu5su2_jd8sdSpbgMWnewNzM1Ub6JDfDjKVz1EVwneo0G6QKV_KHUC_50FpZs6GvsOJnelamO_A" alt=""><figcaption></figcaption></figure>

### Usuarios y Grupos&#x20;

Como en Windows server se pueden configurar usuarios y grupos, los cuales según tengan aplicados los roles tendrán acceso a algunas de las opciones de forma parcial o completa. La lista de los roles de proxmox:

<figure><img src="https://lh4.googleusercontent.com/_rSiczxwpcv6c89mReS59AChxzc_EVRpY5HPnJR9Q5BXD3aW6lH1Oh4uBooxwHBAeEtRuhDQTWGXu4xDrNvRWj7TGIQpJFkuyjHgGKL25gQ1w_I0h8UZV2ggWf9ZRUek1WazMMhU6KyNH41cAJaSIqJgmAK9YUwQJKbHpvAT4vvs6_Kc7_8DqE9j" alt=""><figcaption></figcaption></figure>

Hay que ser consciente que aparte de los usuarios y grupos hay que crear “pools” o conjuntos donde le daremos acceso a los grupos a determinados contenidos ya sean máquinas virtuales, LXC y también acceso a los discos. Esto no significa que después tengas roles que no te permiten acceder a VM o a discos, y otros que puedan modificarlos, todo esto en el mismo grupo.

### Links

* Migrar máquinas virtuales de un sistema VirtualBox o VMWare a Proxmox
  * https://www.calidade.systems/en/2019/08/04/moving-a-virtual-machine-from-virtualbox-to-proxmox/&#x20;
  * https://forum.proxmox.com/threads/importing-virtualbox-vm-to-proxmox-ve-6-x.75610/&#x20;
  * https://pve.proxmox.com/wiki/Migration\_of\_servers\_to\_Proxmox\_VE#Importing&#x20;
  * https://www.youtube.com/watch?v=ATd2fzgLN5g&#x20;
* **Ceph** \
  Ceph File System es un sistema de archivos distribuido libre, está diseñado para el uso con gran cantidad de datos, está muy enfocado para el uso con Big Data.\
  Ceph tiene como objetivo ser POSIX-compatible y completamente distribuido sin ningún punto de fallo.&#x20;
  * https://docs.ceph.com/en/latest/&#x20;
  * https://documentation.suse.com/es-es/ses/5.5/html/ses-all/ceph-operating-services.html&#x20;
  * https://es.wikipedia.org/wiki/Ceph\_File\_System&#x20;
  * https://www.ionos.es/digitalguide/servidores/know-how/que-es-ceph/ VM para instalar en Proxmox para un buen uso:&#x20;
* **TrueNAS**:&#x20;
  * https://www.youtube.com/watch?v=iva4DmOmSTc\&t=184s https://www.youtube.com/watch?v=JzX6c58ydY4
* Instalar Windows 10 con drivers compatibles con Proxmox:&#x20;
  * https://www.youtube.com/watch?v=6c-6xBkD2J4
* Instalar Ubuntu server para uso con docker:&#x20;
  * https://www.youtube.com/watch?v=YR9SNDD8WB4
* Remote gaming:&#x20;
  * https://www.youtube.com/watch?v=fgx3NMk6F54\&t=192s
* Update Proxmox sin suscripción:&#x20;
  * https://www.youtube.com/watch?v=rfK8fc-ccoQ
* Proxmox Backup Server:&#x20;
  * https://www.youtube.com/watch?v=jLBNm0fNIog

### Otros links para ampliar información

* **Spice**:&#x20;
  * http://somebooks.es/spice-protocolo-escritorio-remoto-maquinas-virtuales-proxmox-ve-parte-1/&#x20;
  * http://somebooks.es/spice-protocolo-escritorio-remoto-maquinas-virtuales-proxmox-ve-parte-2/&#x20;
  * http://somebooks.es/spice-acceder-al-escritorio-una-maquina-virtual-proxmox-sin-la-interfaz-grafica/&#x20;
* **Tickets**:&#x20;
  * http://somebooks.es/conectar-al-escritorio-una-maquina-virtual-proxmox-ve-sin-la-interfaz-grafica-usando-tickets/&#x20;
* scripts tickets:&#x20;
  * https://git.proxmox.com/?p=pve-manager.git;a=blob\_plain;f=spice-example-sh;hb=HEAD
* **Usuarios y grupos**:&#x20;
  * https://pve.proxmox.com/wiki/User\_Management&#x20;
  * https://www.youtube.com/watch?v=XlLAbm94\_c8&#x20;
  * http://somebooks.es/crear-nuevas-cuentas-usuario-proxmox-ve/&#x20;
  * http://somebooks.es/crear-conjuntos-proxmox-ve/ http://somebooks.es/asegurar-la-cuenta-root-en-proxmox-ve/&#x20;
* **Otros**:&#x20;
  * http://somebooks.es/usar-ubuntu-para-intercambiar-archivos-con-un-servidor-proxmox-ve-mediante-ssh/  (**rsync ssh**)&#x20;
* http://somebooks.es/eliminar-el-mensaje-no-valid-subscription-al-iniciar-sesion-en-proxmox-ve/ http://somebooks.es/introduccion-la-virtualizacion/&#x20;
* https://www.youtube.com/watch?v=G4SMMhxyNo8 (verificación en dos pasos)

