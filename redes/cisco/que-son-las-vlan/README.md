# ¿Qué son las vlan?

Las VLAN (Virtual Local Area Networks) son una tecnología esencial en la administración de redes empresariales que permiten segmentar una red física en múltiples redes lógicas, ofreciendo ventajas en la gestión, seguridad y eficiencia de la red.&#x20;

* Proveen seguridad, segmentación, flexibilidad&#x20;
* Permiten agrupar usuarios de un mismo dominio broadcast con independencia de su ubicación física en la red.&#x20;
* Permiten agrupar de manera lógica puertos del switch y los usuarios conectados a los mismos se agrupan en grupos de trabajo con interés común.&#x20;
* Es posible asociar usuarios lógicamente con total independencia de la ubicación física incluso a través de redes WAN.&#x20;
* Pueden existir en un solo switch o abarcar varios utilizando los enlaces troncales para transportar el tráfico de las VLAN.&#x20;
* Permite mejorar el rendimiento de las redes al no propagarse las difusiones de un segmento a otro.&#x20;
* Utilizando routers puede existir comunicación entre los VLAN y también se pueden implementar ACL para mantener la seguridad necesaria.

Las VLAN permiten segmentar una red conmutada, de manera que los dispositivos conectados a una misma VLAN tengan conectividad entre sí, pero no con los conectados a otras VLAN. Su estructura se basa en conexiones lógicas y no en conexiones físicas.&#x20;

Dentro de un mismo switch podemos definir diferentes VLAN, pero también podemos definir una misma VLAN que esté ubicada en diversos switches que estén interconectados. Cada uno de los segmentos de red definidos en las VLAN se ha de identificar con un segmento IP diferente.&#x20;

**Ejemplo**: si definimos 8 VLAN, debemos usar 8 segmentos distintos y asignar cada uno a una VLAN. (segmentos de direcciones que utilizan direccionamiento sin clase).

### ¿El por qué de las VLAN?

Si buscamos un poco en la historia de las redes vemos por qué surgieron.&#x20;

*   **1981**&#x20;

    * Las redes Ethernet existían con topología en bus y cable coaxial.&#x20;
    * Siendo redes de difusión si dos equipos transmiten a la vez, se producen colisiones y se desperdicia ancho de banda, por las transmisiones fallidas.&#x20;
    * Conectar varias redes Ethernet era complicado, y aunque se podía utilizar un router para la interconexión, resultaban caros y requería un mayor tiempo de procesado de paquetes.
    * Como solución se inventa el bridge (puente) que permite interconectar dos redes LAN.


*   **1990**&#x20;

    * Se desarrollan los Switches y  se utilizan  para interconectar redes Ethernet lo que permite separar dominios de colisión, aumentando la eficiencia y la escalabilidad de la red.&#x20;
    * Una red tolerante de fallos y con un nivel alto de disponibilidad requiere que se usen topologías redundantes: enlaces múltiples entre switches y equipos redundantes. De esta manera, ante un fallo en un único punto es posible recuperar de forma automática y rápida el servicio.&#x20;
    * La redundancia requiere la habilitación del protocolo Spanning Tree (STP) que permita asegurar que haya activo un único camino lógico para ir de un nodo a otro.&#x20;
    * El principal inconveniente es que los switches centrales se convierten en cuellos de botella, por donde circula la mayor parte del tráfico.


* **Año ???**
  * Se consiguió aliviar la sobrecarga en los switches creando redes virtuales añadiendo una etiqueta a las tramas Ethernet para diferenciar el tráfico.&#x20;
  * Al definir varias redes virtuales cada una tiene su propio Spanning Tree y se pueden asignar diferentes puertos de un mismo switch a cada VLAN.
  * Para unir VLAN que están definidas en varios switches se crean enlaces especiales denominados enlaces troncales (trunk), a través de los cuales fluye tráfico de varias VLAN.
  * Los switches reconocen a qué VLAN pertenece cada trama observando la etiqueta VLAN, definida en la norma IEEE 802.1Q.

### Protocolo Dot1Q

Se trata de un estándar de redes utilizado para implementar VLAN en redes Ethernet. Es un protocolo que permite segmentar una red física en varias redes virtuales, proporcionando: mayor seguridad aislamiento flexibilidad en la gestión de la red.

Dot1Q es compatible con una amplia variedad de dispositivos de red y sistemas operativos, siendo un estándar ampliamente utilizado en redes empresariales.

### VLAN 1

Por defecto, todos los puertos del switch están asociados a la VLAN 1 En caso de tener asociado un puerto a una VLAN y después lo borramos, el puerto volverá a estar asociado automáticamente con la VLAN 1.

Si bien es cierto que la configuración del switch se guarda en el archivo startup-config, la asociación entre los puertos y las VLAN se almacena en el llamado vlan.dat. Si necesitamos eliminar la información de las VLAN usaremos: `delete vlan.dat.`



### Puertos de acceso y puertos troncales

**Puerto de acceso**: Puerto del switch que pertenece a una VLAN. Se utilizan para conectar PC, impresora, teléfono IP, etc. Puerto troncal: Puerto del switch que transmite información de varias VLAN a través de enlaces punto a punto. Puerto que se utiliza para conectar con otro switch o router.

Un puerto de acceso puede pertenecer a una red determinada y al mismo tiempo a una VLAN de voz para uso en telefonía IP.

### Enlaces troncales - trunking

Es una conexión que permite transportar tráfico de varias VLAN a través de un mismo cable físico, lo que permite la comunicación entre Switches y Routers para interconectar varias VLAN.&#x20;

Se establece entre dos Switches o entre un switch y un router y la finalidad es transportar el tráfico de las VLAN de modo que los dispositivos conectados a un Switch se puedan comunicar con otros en otras VLAN conectadas a otros Switches de la red. Usa técnicas de etiquetado para identificar y separar el tráfico de cada VLAN en el cable físico compartido.

Es esencial para implementar una red de VLANs, ya que permite el transporte de tráfico entre diferentes VLANs a través de una única conexión física.



### Etiquetado de trama - IEEE 802.1Q

Es quien identifica el mecanismo de etiquetado de trama de capa 2.

Interconecta Switch, Router y servidores.

Solo los puertos FastEthernet y GigabitEthernet soportan el enlace troncal con etiquetado 802.1q o Dot1q.

Los Switches reconocen la existencia de VLAN a través del etiquetado de trama, identificando el número de la VLAN con independencia del nombre que tengan.

**¿Qué hace este protocolo DOT1Q?**

Adiciona una etiqueta de 4 bytes a cada trama Ethernet, para identificar la VLAN a la que pertenece la trama.

La etiqueta contiene información sobre la VLAN, como el ID de la VLAN, la prioridad de la trama y un campo de control.

Cuando el Switch recibe la trama etiquetada con dot1Q, utiliza esa información de la etiqueta para enrutar los datos a través de la red y asegurar que llegue la información a los dispositivos de la VLAN correspondiente



### VLAN Nativa

Hay casos en los que es necesario que cierto tráfico administrativo no se etiquete a la hora de enviarse por un puerto troncal.&#x20;

Para indicar qué tráfico se enviará a través del puerto troncal sin usar el etiquetado de trama, se define una VLAN concreta a la que no se le aplica el etiquetado al pasar por un puerto troncal. Esta VLAN se conoce como nativa.&#x20;

Si no definimos una VLAN como nativa el switch asume que la nativa será la VLAN 1 y todo el tráfico que se genere a través del mismo estará sin etiquetar.

Esta VLAN 1 no se puede eliminar

* Es la VLAN predeterminada en el estándar IEEE 802.1Q.&#x20;
* Es la VLAN que no tiene asignada una etiqueta de VLAN en las tramas de datos.&#x20;
* Por defecto, es la VLAN 1 para la mayoría de los Switches. Todos los puertos del switch se asignan automáticamente a la VLAN 1, a menos que sean configurados para pertenecer a otra VLAN. Esto significa que cualquier dispositivo conectado a un puerto que no ha sido asignado a una VLAN específica se incluirá automáticamente en la VLAN nativa.&#x20;
* Se utiliza para el tráfico que no se ha etiquetado con una VLAN específica. Por ejemplo, el tráfico generado por un dispositivo que no está configurado para enviar tráfico con etiquetas de VLAN, como una impresora antigua, se incluirá automáticamente en la VLAN nativa.&#x20;
* Se asigna siempre a aquellos dispositivos que no han sido etiquetados con una VLAN específica. Por lo tanto, es importante considerar la seguridad de la VLAN nativa, ya que puede ser vulnerable a ataques si no se configura adecuadamente.

### Etiquetado de VLAN

Al definir una VLAN en el Switch, se asocia un número de 12 bits para su representación, lo supone valores entre 0 y 4095. Aunque los valores 0 y 4.095 no se pueden utilizar. Como puede resultar poco práctico, se asocia una etiqueta que permite identificar a cada VLAN junto con su número de identificación.



### VLAN de datos

Aquellas que permiten separar los tráficos generados por los dispositivos. Son VLANs en las que se conectarán dispositivos para el envío de tráfico genérico: datos, vídeo o audio. Y por defecto, la VLAN 1 es una VLAN de datos.

En redes en las que se utilizan teléfonos IP se pueden asociar los puertos donde se han conectado los teléfonos IP en una VLAN especial denominada VLAN de voz. Este uso especial permite que en un mismo puerto del switch puedan coexistir una VLAN de voz y una VLAN de datos para ahorrar el uso de puertos.



### VLAN de voz

Un teléfono envía voz sobre IP mediante el protocolo VoIP (Voice over IP). Al ser un teléfono IP un dispositivo final, necesita una conexión de red para conectarse a los demás dispositivos de la empresa. Eso supone que necesitará un puerto de red en el switch para conectarse, de manera similar a como se conectaría un PC o una impresora a la red.

Para simplificar la conexión, los teléfonos IP permiten conectar el PC al teléfono, y éste a la red. De esta manera, solo se necesita un puerto del switch para conectar ambos dispositivos, simplificando la topología de red física y el número de switches a instalar.

* Diseñada para el tráfico de voz en una red de telefonía IP (VoIP). Se utilizan para separar el tráfico de voz de otros tipos de tráfico en una red y proporcionar una calidad de voz mejorada.&#x20;
* Su implementación permite configurar los switches para que los paquetes de voz se clasifiquen y se asignen a una VLAN de voz específica. Lo que permite priorizar los paquetes de voz en la red y se les otorgue un ancho de banda que garantice la calidad de la llamada.
* &#x20;Ayudan a mejorar la seguridad de la red, al separar el tráfico de voz del de datos, dado que se pueden aplicar políticas de seguridad específicas para proteger la red contra amenazas.

En resumen, la implementación de una VLAN de voz es una buena práctica para mejorar la calidad de voz y la seguridad en una red de telefonía IP.

Para etiquetar tráfico VoIP que necesita:&#x20;

* Ancho de banda mínimo para garantizar la calidad del audio enviado.&#x20;
* Prioridad de ese tráfico con respecto a otros tráficos de datos. El consumo de tráfico enviado por VoIP en un teléfono es de unos pocos kilobits, por lo que no afectará al rendimiento global del PC conectado al teléfono.&#x20;
* Prioridad de los tráficos utilizando mecanismos de QoS (Quality of Service) garantizando una latencia inferior a 150 milisegundos en la transmisión.

### Clasificación de las VLAN

Las VLAN se clasifican en:

* **VLAN de nivel 1 (por puerto) o port switching** - especifica qué puertos del switch pertenecen a la VLAN, los miembros de dicha VLAN son los que se conecten a esos puertos. No permite la movilidad de los usuarios, habría que reconfigurar las VLAN si el usuario se mueve físicamente. Es la más común.&#x20;
* **VLAN de nivel 2 por direcciones MAC** - se asignan hosts a una VLAN en función de su dirección MAC. Tiene la ventaja de que no hay que reconfigurar el dispositivo de conmutación si el usuario cambia su localización, es decir, se conecta a otro puerto de ese u otro dispositivo. Inconveniente: hay que asignar los miembros uno a uno y si hay muchos usuarios puede ser agotador.&#x20;
* **VLAN de nivel 3 por tipo de protocolo**. Por el tipo de protocolo. Por ejemplo, se asociaría VLAN 1 al protocolo IPv4, VLAN 2 al protocolo IPv6.&#x20;
* **VLAN de nivel 4 por direcciones de subred (subred virtual)**. Son los paquetes, y no las estaciones, quienes pertenecen a la VLAN.
* VLAN de niveles superiores - Se crea una VLAN para cada aplicación:  FTP, multimedia, correo electrónico.

La pertenencia a una VLAN puede basarse en una combinación de factores como puertos, direcciones MAC, subred, hora del día, forma de acceso, condiciones de seguridad del equipo.

