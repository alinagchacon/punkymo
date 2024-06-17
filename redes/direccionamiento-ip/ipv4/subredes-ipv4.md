# Subredes IPv4

Las ventajas de la creación de subredes están en básicamente en hacer más eficiente la administración y el enrutamiento.

* **Evita difusiones innecesarias**:\
  Los PC conectados a una red, normalmente envían información a cualquier computadora que está en su red, lo que se conoce como una difusión. Las difusiones son causadas por virus y malware, así como muchos programas legítimos. En redes más pequeñas (por ejemplo, de menos de 50 personas), esto puede no ser un problema, sin embargo, en organizaciones con cientos o miles de usuarios, esto puede hacer rápidamente más lenta la red.
* **Aumenta la seguridad**:\
  La mayoría de los dispositivos de seguridad de red funcionan mediante la evaluación de tráfico entre redes. Al poner los recursos sensibles en la misma subred que cualquier otro usuario, se hace más difícil implementar medidas de seguridad. La separación de las funciones vitales en subredes permite implementar medidas de seguridad como firewalls.
* **Simplifica la administración**: \
  una organización tiene diferentes departamentos que requieren acceso a diferentes tipos de recursos. Si los departamentos de contabilidad y limpieza se encuentran en la misma subred, por ejemplo, entonces las restricciones de acceso tienen que ser controladas en base de nodo-a-nodo. Pero cuando los dos departamentos se colocan en subredes separadas, entonces las opciones de seguridad se pueden aplicar sobre la base de esas subredes.
* **Controla el crecimiento**: \
  Al planificar una red, puedes controlar el número de máscaras de subred disponibles y cuántos nodos estarán disponibles para cada subred.

### Segmentando las redes

Las direcciones IP que podemos utilizar para redes locales (internas) son direcciones IP privadas. Sabemos que existen segmentos privados en cada una de las clases  A,  B y C que podemos utilizar para asignar a los dispositivos.&#x20;

**¿Cuántos segmentos de red privados podemos utilizar?**&#x20;

* Solamente hay una red privada de clase A: la 10.0.0.0/8.&#x20;
* De clase B tenemos 16 redes privadas: desde la IP 172.16.0.0/16 hasta la 172.31.0.0/16.&#x20;
* De clase C tenemos 256 redes privadas: desde la 192.168.0.0/24 hasta la 192.168.255.0/24.

Esto nos da un total de 273 redes privadas, pero entonces imagínate que nuestra organización necesite más de 273 segmentos de red?&#x20;

Pues que tenemos que cambiar el modo de hacer las cosas: nos tenemos que pasar al llamado direccionamiento sin clase o <mark style="color:blue;">`classless`</mark>, de manera que desaparezcan los conceptos de clases asociados a una determinada máscara de red.

Existen diferentes maneras de optimizar el uso de direccionamiento sin clase:&#x20;

* las <mark style="color:blue;">`subredes`</mark> IP,&#x20;
* las redes con máscara de red de <mark style="color:blue;">`longitud variable`</mark> (también conocidas como <mark style="color:blue;">`VLSM`</mark> por sus siglas en inglés)&#x20;
* las <mark style="color:blue;">`superredes`</mark>, que permiten agrupar conjuntos de redes en redes de mayor tamaño que permitirán, sobre todo, aprovechar el direccionamiento en las tablas de enrutamiento.

### Subredes IPv4

Una subred nos permite dividir una red inicial de una determinada clase en diferentes redes. Estas redes tendrán en común la red principal, sin embargo utilizarán un número determinado de bits de host (aquellos situados más a la izquierda) para representar las subredes que necesitemos con esa dirección de base.

Dicho de otra manera, creamos subredes "pidiendo" bits a la parte del host para completar la parte de bits que representan la red. Por ejemplo,

* Con 1 bit podemos crear 2 redes.&#x20;
* Con 2 bits podemos crear 4 redes.&#x20;
* Con 3 bits, 8 redes.&#x20;
* Con 4 bits, 16 redes



### Comunicación entre subredes

* Se necesita un router para que los dispositivos en diferentes redes y subredes puedan comunicarse.
* Cada interfaz del router debe tener una dirección de host IPv4 que pertenezca a la red o a la subred a la cual se conecta la interfaz del router.
* Los dispositivos en una red y una subred utilizan la interfaz del router conectada a su LAN como gateway predeterminado.

<figure><img src="../../../.gitbook/assets/image (359).png" alt=""><figcaption><p>Tomado de CCNA</p></figcaption></figure>

La planificación de las subredes requiere la toma de decisiones sobre el tamaño, la cantidad de hosts por subredes, así como la forma de asignar las IP a los hosts.

Para crear subredes tenemos que tomar bits prestados de la porción que identifica al host. Si tenemos la dirección de red siguiente:

| Red de clase C    | 192.168.1.0 / 24 | 192.168.1.0000 0000   |
| ----------------- | ---------------- | --------------------- |
| Máscara de subred | 255.255.255.0    | 255.255.255.0000 0000 |

Vamos a suponer que pedimos prestado un bit a la porción del host. Esto implica que con 1 bit = 2^1 = 2 subredes.

Originalmente es una IP de clase C, pero al pedirle prestado 1 bit, en lugar de tener 2^8 = 255 tendremos 2^7 = 128\
\


| <p>Subred 0</p><p></p>                          | <p>192.168.1.0 / 25</p><p>192.168.1.1 / 25</p><p>………………..</p><p>192.168.1.127 /25</p>                              | <p>255.255.255.128</p><p>(Adaptada)</p> |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | --------------------------------------- |
| <p></p><p></p><p>Subred 1</p><p></p><p><br></p> | <p>192.168.1.128 / 25</p><p>192.168.1.129 / 25</p><p>192.168.1.130 / 25</p><p>…………………</p><p>192.168.1.255 / 25</p> | <p>255.255.255.128<br>(Adaptada)</p>    |

#### Ejemplo de 2 subredes en Cisco

A continuación os dejo con un ejemplo de subredes en Cisco.

<figure><img src="../../../.gitbook/assets/image (360).png" alt=""><figcaption><p>Subredes en Cisco</p></figcaption></figure>

#### ¿Y cuántos host hay por subred?

Si en el ejemplo se ha tomado 1 bit = 2 ^1 = 2 subredes, lo que implica que nos quedan 7 bits para representar los hosts, esto es:

2^7 = 128 hosts por subred

### &#x20;Ejemplo de 4 subredes en Cisco

Si tomamos 2 bits prestados a la parte de host, entonces tendremos 2^2 = 4 subredes como se muestra en la imagen a continuación:

<figure><img src="../../../.gitbook/assets/image (361).png" alt=""><figcaption></figcaption></figure>

2^6 = 64 hosts por subred

192.168.1.0

255.255.255.192

### &#x20; Ejemplo de 8 subredes

Si se toman 3 bits serían 2^3 = 8 subredes

<figure><img src="../../../.gitbook/assets/image (362).png" alt=""><figcaption><p>Tomado de </p></figcaption></figure>

Y tendríamos 2^5 = 32 hosts por subred, como se puede ver en la imagen de Cisco siguiente:

<figure><img src="../../../.gitbook/assets/image (363).png" alt=""><figcaption><p>8 subredes </p></figcaption></figure>

