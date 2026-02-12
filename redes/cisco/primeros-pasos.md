---
description: Router Cisco
---

# Primeros pasos

En este apartado vamos a ver como tenemos que hacer para acceder por primera a vez a configurar un router Cisco.&#x20;



### MobaExterm

¿Por qué este software?

En primer lugar porque me lo recomendó hace un tiempo un ex-docente y consultor de ciberseguridad al que tengo mucha admiración: [Gabriel Martí](https://www.linkedin.com/in/gabimarti/). En segundo lugar, porque es una alternativa mucho más eficiente y completa que Putty. Por tanto, cuando se trata de la gestión de servidores y el acceso remoto, `Mobaxterm` es una herramienta imprescindible.&#x20;

Mobaexterm es un conjunto de herramientas de administración de servidores que combina una interfaz gráfica de usuario (GUI) y un cliente de terminal en una sola aplicación, válido en sistemas operativos Windows y ofrece una amplia gama de funcionalidades que facilitan la gestión de servidores de forma remota.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Este software:

* Permite conectarnos por acceso remoto utilizando protocolos como SSH, Telnet, RDP y VNC, desde  una interfaz única. Esto facilita la gestión de varios servidores sin tener que abrir más de una ventana de terminal o utilizar diferentes herramientas.
* Realiza transferencia de archivos&#x20;
* Ejecuta  comandos
* Ofrece un terminal avanzado que combina las características de un cliente de terminal con una interfaz gráfica de usuario (GUI)
* &#x20;Una de las características del terminal de Mobaxterm es que permite el resaltado de sintaxis, autocompletado de comandos, múltiples pestañas, etc.
* Soporta protocolos como SFTP y SCP.
* Otra de las características  de Mobaxterm es su servidor X11 integrado, que permite ejecutar aplicaciones gráficas de forma remota y visualizarlas en el entorno local.

### Conexión por consola

Cuando nos llega un dispositivo de red "nuevo" como puede ser un Router lo ideal es conectarnos por consola con un cable de consola USB como el que se muestra a continuación.

<figure><img src="../../.gitbook/assets/image (3).png" alt="" width="239"><figcaption></figcaption></figure>

Conectamos el cable de consola al puerto de consola del dispositivo, en este caso lo haré a un Router Cisco y por el puerto USB a un PC donde tengamos instalado MobaExterm.&#x20;

A continuación buscamos el administrador de dispositivos para asegurarnos cuál es el puerto de comunicación que se ha habilitado. Como vemos en la imagen, es el COM3.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>



Ahora si que nos vamos al Mobaexterm, nos vamos a la pestaña "Session" y buscamos  "Serial"&#x20;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Nos encontraremos con una pantalla como la siguiente donde tenemos que seleccionar el mismo puerto COM3 que se ha activado en nuestro PC y teniendo en cuenta las especificaciones del dispositivo tendremos que seleccionar unas características u otras como es el caso de la velocidad de comunicación del puerto, que en el caso del Router Cisco, es de 9600 bps.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Una vez conseguida la conexión con el dispositivo ya tendrá el acceso al terminal para configurarlo.  La siguiente imagen muestra un pantallazo del Router de la serie 2900 que son parte de la generación de routers de servicios integrados (ISR) de Cisco, que fueron diseñados para proporcionar conectividad de alto rendimiento y servicios integrados. La serie incluye modelos como el Cisco 2901, 2911, 2921 y 2951.





<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>



La configuración del Router la almaceno en la flash  haciendo:&#x20;

```
copy running-config flash0:/router-config-backup 
```

Por tanto, para restablecer la configuración tenemos que volcar el archivo en el running-config:

```
copy flash0:/router-config-backup running-config
```

Me falta explicar algunos detalles interesantes para entender el por qué de todo esto. Pero te lo debo.

### Link

* [https://tecno-simple.com/que-es-mobaxterm-y-para-que-sirve-alternativas/](https://tecno-simple.com/que-es-mobaxterm-y-para-que-sirve-alternativas/)
* [https://es.wikipedia.org/wiki/Cable\_de\_consola](https://es.wikipedia.org/wiki/Cable_de_consola)
* [https://www.cisco.com/c/en/us/support/docs/routers/7000-series-routers/12223-14.html](https://www.cisco.com/c/en/us/support/docs/routers/7000-series-routers/12223-14.html)

