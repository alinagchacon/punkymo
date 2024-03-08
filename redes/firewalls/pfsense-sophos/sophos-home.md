# Sophos home

### Descarga del software

Nos vamos a la página: [https://www.sophos.com/es-es/free-tools/sophos-xg-firewall-home-edition](https://www.sophos.com/es-es/free-tools/sophos-xg-firewall-home-edition) y nos descargamos la versión home de Sophos.

<figure><img src="../../../.gitbook/assets/image (288).png" alt=""><figcaption></figcaption></figure>

Nos pedirá rellenar un breve formulario de suscripción y nos remitirá a la sección de descarga, como se visualiza en la imagen siguiente:

<figure><img src="../../../.gitbook/assets/image (289).png" alt=""><figcaption><p>Descarga de Sophos home edition</p></figcaption></figure>

Al clicar sobre el botón de Download nuevamente nos pide rellenar otro formulario y con esto, finalmente comienza la descarga del software.

### Instalación en VirtualBox

A la hora de hacer la instalación tenemos que ver los parámetros de configuración como tipo de sistema, arquitectura, ram, etc. En mi caso he utilizado:

| Características de la VM |                   |
| ------------------------ | ----------------- |
| RAM                      | 4GB               |
| Disco                    | 20GB              |
| Adaptador de red 1       | Red Interna (LAN) |
| Adaptador de red 2       | NAT (WAN)         |

Tengamos en cuenta que Sophos busca en el primer adaptador la red LAN y en el segundo la WAN aunque esto lo podemos configurar.

Una vez comienza el proceso de instalación, nos pide reiniciar la VM y comienza una segunda parte.

<figure><img src="../../../.gitbook/assets/image (293).png" alt="" width="542"><figcaption><p>Segunda parte de la instalación</p></figcaption></figure>

El tiempo que tarda la instalación es mínimo, así que una vez finaliza nos pide la contraseña para acceder. Esta contraseña por defecto es: `admin`.

<figure><img src="../../../.gitbook/assets/image (294).png" alt="" width="538"><figcaption><p>Sophos home instalado en VM</p></figcaption></figure>

### Acceso al sistema

Lo primero que hago es revisar los adaptadores de red y las direcciones IP que tiene cada uno para poder acceder a través de un navegador. Para ello escribimos 1 clicamos al Enter y volvemos a seleccionar la opción 1 en el siguiente menú.

<figure><img src="../../../.gitbook/assets/image (296).png" alt="" width="530"><figcaption><p>Menú de configuración de Sophos</p></figcaption></figure>

Si hemos seleccionado correctamente los adaptadores de red podremos ver la IP, máscara y demás características seleccionadas por defecto. Fijáos que la IP es estática en este caso.

<figure><img src="../../../.gitbook/assets/image (297).png" alt="" width="530"><figcaption><p>Adaptador de red LAN con IP 172.16.16.16</p></figcaption></figure>

Nota: En el caso de seleccionar el adaptador de red interna para la conexión con la LAN nos obliga de cierto modo a utilizar otra VM que esté conectada al mismo adaptador de red para poder configurar el firewall desde navegador. Otra opción sería utilizar adaptador solo anfitrión con lo cual podemos acceder desde el navegador de nuestro host.

Y para el segundo adaptador de red tenemos la IP 10.0.3.15 porque seleccioné el adaptador NAT para la conexión con la WAN. Observad que la IP es dinámica.

<figure><img src="../../../.gitbook/assets/image (298).png" alt="" width="530"><figcaption><p>Adaptador de red WAN</p></figcaption></figure>

Para acceder desde el navegador al dashboard de administración de Sophos, he utilizado una VM de Debian y utilizando la IP de la LAN accedemos al dashboard. Esto es:&#x20;

```
https://172.16.16.16:4444
```

<figure><img src="../../../.gitbook/assets/image (300).png" alt="" width="563"><figcaption></figcaption></figure>

### Configuración básica de Sophos

En este punto es que podemos realmente comenzar el proceso de configuración de Sophos. Lo primero que nos pide es:&#x20;

* nueva contraseña de acceso
* nombre para el dispositivo y zona horaria
* registrar el firewall (podemos hacerlo más adelante) pero al rellenar el formulario para poder hacer la descarga del software nos llegó a nuestro buzón de correos un email con el número de serie asignado y es justo lo que necesitamos en este paso.

Una vez hecho esto ya tendríamos la configuración básica del firewall y podríamos comenzar con aspectos más avanzados.

#### LAN

En este punto podemos modificar el puerto de conexión con la red LAN así como la IP de la interfaz de red del firewall (para la LAN). Igualmente podemos modificar el rango de direcciones que el firewall va a otorgarle a los dispositivos que se conecten a la misma. Por defecto nos viene el rango de conseción de la 172.16.16.17 a la 172.16.16.254 teniendo en cuenta que el prefijo de la red es 24, o sea: 255.255.255.0.

#### WAN



<mark style="color:red;">To be continued ....</mark>





### Links

* [https://support.home.sophos.com/hc/en-us/articles/115003571389-Protecting-additional-computers-with-Sophos-Home](https://support.home.sophos.com/hc/en-us/articles/115003571389-Protecting-additional-computers-with-Sophos-Home)
*
