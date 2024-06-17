# Métodos de acceso  a la red

Tanto en las redes LAN como WLAN (las inalámbricas) es necesario hacer un control del tráfico de los datos en la red. Ambos tipos de redes son ejemplos de lo que se denomina acceso múltiple.&#x20;

¿Qué significa esto? Pues que puede tener dos o más dispositivos finales que intentan acceder a la red simultáneamente.

Existen dos métodos básicos de control de acceso para medios (físico o inalámbrico) compartidos:

1. **Acceso basado en la contención**

* **CSMA/CD - Carrier sense multiple access with collision detection:** utilizado en redes Ethernet de topología de bus heredada.
* **CSMA/CA - Carrier sense multiple access with collision avoidance** utilizado en redes LAN inalámbricas.

Cuando se trata de **CSMA**/CD, todos los nodos funcionan en semidúplex, compitiendo por el uso del medio, aunque solo un dispositivo puede enviar a la vez. &#x20;

Si se da el caso en que dos dispositivos transmiten simultáneamente, se producirá una colisión. Para las LAN Ethernet heredadas, ambos dispositivos detectarán la colisión en la red.&#x20;

La tarjeta de red o NIC compara los datos transmitidos con los datos recibidos y observa que  se dañarán y deberán reenviarse.

<figure><img src="../../.gitbook/assets/image (121).png" alt=""><figcaption><p>Tomado de IONOS</p></figcaption></figure>

**Ejemplo de uso**:

* LAN inalámbrica (IEEE 802.11, CSMA/CA)
* LAN Ethernet de topología de bus heredada (IEEE 802.3, CSMA/CD)
* LAN Ethernet heredada usando un Hub (IEEE 802.3,  CSMA/CD)

Cuando se trata de CSMA/CA se utiliza un método similar al para detectar si el medio está libre o no, aunque utiliza técnicas adicionales, puesto que en entornos inalámbricos, los dispositivos no detectan las colisiones.&#x20;

Aunque no detecta las colisiones intenta evitarlas poniendo un tiempo de espera antes de transmitir los datos. Los dispositivos que transmiten incluye el tiempo de duración que necesita la transmisión. Todos los demás dispositivos  reciben esa información con lo cual  pueden conocer el tiempo durante el cual el medio no estará disponible.



2\. **Acceso controlado**

En este caso, cada nodo tiene su propio tiempo para usar el medio, a partir de un token que circula por el medio. Estos tipos deterministas de redes son ineficientes porque un dispositivo debe esperar su turno para acceder al medio.&#x20;

Un ejemplo de esto es el llamado Token Ring que es una tecnología de red desfasada dado que hoy en día quien domina en el mundo de las redes por cable es ETHERNET.&#x20;

Aún así es interesante y es útil conocer los procedimientos antiguos para comprender las redes actuales y cuál ha sido su evolución y por qué.&#x20;

Este tipo de redes <mark style="color:blue;">`token ring`</mark> no son en realidad redes en anillo. De hecho, el anillo se produce de manera lógica y no física.

Esta topología token ring está compuesta de **unidades de acceso a múltiples estaciones (MAU - Multistation Access Units)** que permiten la conexión de los equipos en forma de estrella.&#x20;

El distribuidor es un punto central conectado con todos los nodos de la red y no hay conexión directa entre los equipos.

Esto implica que se habla de un anillo lógico sobre una **estructura física de estrella**, ya que la transmisión de datos se produce, a nivel abstracto, en forma de anillo. Si bien los datos siempre son llevados hasta la MAU, desde allí no se envían a ningún ordenador concreto, sino simplemente al siguiente en el orden fijado.

Te recomiendo los siguientes enlaces:\


1. [https://www.muyinteresante.es/tecnologia/articulo/como-funciona-una-red-token-ring-241573560345](https://www.muyinteresante.es/tecnologia/articulo/como-funciona-una-red-token-ring-241573560345)
2. [https://www.ionos.es/digitalguide/servidores/know-how/token-ring/](https://www.ionos.es/digitalguide/servidores/know-how/token-ring/)
