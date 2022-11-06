# Subredes IPv4

### Segmentando las redes

Las direcciones IP que podemos utilizar para redes locales (internas) son direcciones IP privadas. Sabemos que existen segmentos privados en cada una de las clases  A,  B y C que podemos utilizar para asignar a los dispositivos.&#x20;

¿Cuántos segmentos de red privados podemos utilizar?&#x20;

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

<mark style="color:red;">FALTA</mark>
