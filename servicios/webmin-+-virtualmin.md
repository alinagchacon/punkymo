---
description: Alternativa a Plesk
---

# Webmin + Virtualmin

Buscando una alternativa a Plesk que fuera gratuita y analizando los pro y contra de diferentes opciones me decanté por Webmin con Virtualmin. Webmin ya la conocía y me parecía interesante de por sí así que opté por esta alternativa.

Se trata de una herramienta que nos permite administrar cualquier máquina Linux desde una interfaz web. Desde un panel de control podemos administrar diferentes aspectos de la configuración del sistema como son: firewall, MySQL, Email, DNS, DHCP, paquetes instalados, etc.

Para que funcione como alternativa a cPanel debemos instalar otra herramienta: [Virtualmin](https://www.virtualmin.com/).

Webmin y Virtualmin tienen un funcionamiento modular,  que se basa en el uso de plugins que van añadiendo funcionalidades extras. Esto nos permite personalizar la instalación. Por tanto:

* **Webmin** es una herramienta de configuración de sistemas accesible vía web. Diseñado para sistemas Unix, GNU/Linux y OpenSolaris. Nos permite configurar servicios del tipo: servidor web Apache, PHP, MySQL, DNS, Samba, DHCP, entre otros.
* **Virtualmin** es un panel de control de alojamiento web potente y flexible para sistemas Linux y BSD. Disponible en  dos versiones: una versión de código abierto y en otra con más funciones con soporte premium, siendo la solución completa para la gestión de alojamiento web virtual.

### Instalación de Webmin

La instalación de Webmin+Virtualmin la voy a hacer en un contenedor lxc en Proxmox. Dicho Proxmox es una VM en VirtualBox  conectada a red NAT, por lo que tiene que una IP: 10.0.2.15. Para acceder a Proxmox he utilizado reenvío de puertos. Esto me permite poder acceder utilizando la IP del equipo host: https://192.168.1.83.

Comencemos el proceso de instalación.

* Instalar un contenedor lxc con debian 11&#x20;

Este es un proceso rápido y sencillo siempre y cuando tengamos bien definidas las características de hardware a utilizar. En general, para hacer pequeñas pruebas que no consumen casi cpu, memoria y espacio en disco, podemos dejar los valores por defecto.&#x20;

> En caso de necesitar instalar el contenedor para ponerlo en producción (también como prueba) debemos tener en cuenta la cpu, ram y espacio en disco del equipo anfitrión y del asignado a Proxmox.

* Actualizamos los paquetes:&#x20;

```
apt update | apt upgrade
```

* Descargamos el script que nos permite la instalación de Webmin:

```
curl -o setup-repos.sh 
https://raw.githubusercontent.com/webmin/webmin/master/setup-repos.sh
```

* Ejecutamos el script para agregar el repositorio de Webmin al sistema:&#x20;

```
sh setup-repos.sh
```

* Para descargar el paquete instalador de webmin:&#x20;

```
apt-get install –install-recommends webmin
```

* Verificar que tenemos el paquete webmin listo para instalar con:&#x20;

```
apt update
```

* Instalar webmin

```
apt install webmin -y
```

* Verificar que el paquete esté instalado:&#x20;

```
apt list --installed
```

Una vez que tenemos Webmin instalado podemos acceder al servicio desde el navegador. Para ello ponemos la IP del contenedor: https://192.168.1.13:10000 como se muestra en la imagen a continuación.

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="563"><figcaption><p>Accediendo a Webmin desde el equipo anfitrión</p></figcaption></figure>

El usuario para acceder sería el mismo usuario del sistema que tengamos instalado. Como en este caso, no hemos creado ningunno, tendríamos qu conectarnos como root, algo que no es para nada seguro.  Por tanto, crea un usuario para acceder desde Webmin.

### Instalación de Virtualmin

Como decíamos, se trata de un panel de control de sitios web y alojamiento de dominios que permite crear y administrar diversos dominios. Está basado en Webmin y es una buena alternativa a cPanel y Plesk. Existen dos versiones:&#x20;

* Virtualmin GPL: software gratuito y de código abierto.
* Virtualmin Profesional: de pago

Ambas versiones de Virtualmin permiten crear servidores virtuales con usuarios, buzones de correo, entornos de desarrollo de aplicaciones web, aplicaciones web, establecer cuotas y reglas de cuentas e instancias de servidor web, servidor de DB, etc. Es compatible con servidores web como Apache o httpd.

Veamos el proceso de instalación.

### Instalación de Virtualmin

Para la instalación de esta herramienta se recomienda:

* La instalación automatizada requiere un sistema operativo compatible recién instalado
* 1 GB RAM, mejor si es más
* 1 GB de espacio libre en disco, más para los datos del dominio
* Un nombre de dominio con registros DNS que apunten a la dirección IP de su servidor

Del mismo modo que utilizamos el proceso de instalación automatizado mediante un script, en esta ocasión vamos a hacer lo mismo: utilizar el script **virtualmin-install.sh**.&#x20;

Se recomienda utilizar una instancia de versión de servidor mínima del S.O, dado que el script virtualmin-install.sh instalará los paquetes adicionales que se requieran.&#x20;

La forma más rápida de iniciar la instalación de Virtualmin GPL es ejecutando el siguiente comando preparado previamente:

```
sudo sh -c "$(curl -fsSL https://software.virtualmin.com/gpl/scripts/virtualmin-install.sh)" -- --bundle LAMP
```

Hay algunas opciones disponibles para instalar como puede ser la instalación de Nginx en lugar de Apache.&#x20;

Al ejecutar el script de instalación con el indicador --help se puede obtener una lista de opciones disponibles sobre los paquetes de instalación que se hayan disponibles y el modo de instalación.

Como se puede ver en la imagen siguiente, el script de instalación nos hace algunas preguntas.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Iniciando la instalación de Virtualmin</p></figcaption></figure>

\
La instalación puede tardar unos minutos. Durante el proceso podremos ver que, en mi caso, aparece Postfix pre-instalado, de la propia instalación de Webmin.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>Instalando Virtualmin</p></figcaption></figure>

Una vez que finaliza el proceso de instalación ya podremos acceder a Webmin y veremos que tenemos dos pestañas: una para las configuraciones específicas de Webmin y la otra para Virtualmin. &#x20;

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Vwebmin + Virtualmin</p></figcaption></figure>



La parte realmente interesante comienza a partir de ahora: las configuraciones necesarias.

### Links

* [https://itslinuxguide.com/install-webmin-debian/](https://itslinuxguide.com/install-webmin-debian/)
* [https://www.virtualmin.com/docs/installation/guides/](https://www.virtualmin.com/docs/installation/guides/)

\
\
