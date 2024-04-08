# Datos - codificación

Los datos son una mezcla de diferentes tipos: texto, números, imagen, audio, video.

* Utilizamos representaciones uniformes de todos los tipos de datos
* Cuando se entran datos a un dispositivo,al ser usados y almacenados, éstos se transforman según la representación uniforme establecida.
* Se le denomina patrón de bits a esa representación o formato universal

La representación uniforme de datos en un equipo se refiere a un formato estándar en el que la información se organiza y almacena de manera consistente para facilitar su manipulación y procesamiento.

## Bits

Un bit es la unidad mas pequeña de datos que puede almacenarse en un dispositivo: 0 o 1. Un bit no sirve para representar los datos. Se necesita almacenar número más grandes, textos, gráficos, etc. Se hace necesario el uso los patrones de bits, secuencia o cadenas de bits.

## Bytes

Un patrón, secuencia o cadenas de bits con una longitud de 8 bits es un byte Cualquier idioma se puede definir como una secuencia de sḿbolos usados. Se puede representar cada símbolo con un patrón de bits ¿Cuántos bits podemos necesitar en una secuencia para representr un símbolo en un idioma dado?

## Patrones de bits

* Si tenemos un patrón de 2 bits tendríamos 4 combinaciones posibles: 00, 01, 10 y 11
* Si tenemos un patrón de 3 bits tendríamos 8 combinaciones posibles: 000, 001, 010, 011,00, 101, 110 y 111
* Cada combinación puede representar un símbolo

## Códigos

Se han diseñado distintos tipos de secuencias de patrones para poder representar los caracteres de un texto. Al proceso de de representar los caracteres o símbolos se le conoce como codificación.

{% embed url="http://www3.uacj.mx/CGTI/CDTE/JPM/Documents/IIT/sistemas_numericos/representacion/index.html" %}

## ASCII

Es el estándar para la representación de caracteres en cualquier dispositivo electrónico. Código de caracteres basado en el alfabeto latino. Creado en 1963 por ANSI como una evolución de los códigos utilizados en telegrafía. En 1967 se publicó En 1986 tuvo la última actualización Utiliza 7 bits para representar los caracteres. Casi todos los sistemas informáticos actuales usan el ASCII o una extensión compatible.&#x20;

{% embed url="https://ascii.cl/es/" %}

### ASCII Extendido

Para hacer que el tamaño de cada patrón sea de 1 byte (8 bits), a los patrones de bits ASCII se les aumenta un cero más a la izquierda. Cada patrón cabe fácilmente en un byte de memoria.

Los caracteres ASCII van:&#x20;

* De 0 a 127 en decimal De la “A – Z” (del 65 al 90)&#x20;
* De la “a – z” (del 97 al 122)

Algunas representaciones de caracteres:&#x20;

* Alt + 32 - espacio&#x20;
* Alt + 64 - arroba desde el teclado alfanumérico&#x20;
* Alt + 10 – cambio de línea&#x20;
* Alt + 27 - escape

Muchos de los caracteres de control se usaban para controlar protocolos de transmisión de datos, como ocurre en comunicación:&#x20;

* <mark style="color:red;">**ACK**</mark>**nowledge**: acuse de recibo -  mensaje que el destino envía al origen para confirmar la recepción de un mensaje.&#x20;
* Se definen también diferentes ACK con información más compleja como peticiones de reenvío de determinadas tramas o información sobre incidencias entre otros.&#x20;
* Los mensajes ACK se utilizan en la mayoría de las capas del OSI pero son esenciales en la capa 2 y 3.
* <mark style="color:red;">**NACK**</mark> (negative acknowledgement o acuse de recibo negativo – se envía para informar que en la recepción de una trama de datos ha habido un error.  Es el contrario de  ACK.&#x20;
* <mark style="color:red;">**SYN**</mark> – sincronía en espera dado que no hay bits de start, stop o paridad presentes en los sistemas de comunicación en serie síncronos, hubo que establecer una serie de caracteres para el reconocimiento de los caracteres a sincronizar en el envío de información.

## Unicode

Por la necesidad de disponer de un código con mayores capacidades. Se creó una coalición de fabricantes de hardware y software que desarrolló un código que utiliza 16 bits y puede representar hasta 65 536 (216) símbolos. Distintas secciones del código se asignan a los símbolos de distintos idiomas en el mundo.

{% embed url="https://unicode-table.com/es" %}

{% embed url="https://docs.oracle.com/cd/E26921_01/html/E27143/glmgn.html" %}

## ISO

La ISO diseñó un código que utiliza patrones de 32 bits.

**ISO 8859-1.** Es una norma de la ISO que define la codificación del alfabeto latino. Incluye letras acentuadas, ñ, ç) y especiales como ß, Ø) que son necesarios para la escritura de idiomas europeos como: alemán, español, catalán, euskera, francés,  inglés, islandés, italiano,  neerlandés, noruego, portugués, etc.



## UTF8

* Diseñado en 1992&#x20;
* Formato de codificación de caracteres Unicode e ISO 10646 de longitud variable.&#x20;
* Es una de las 3 posibilidades de codificación reconocidas por Unicode y Web.&#x20;
* Es capaz de representar todos los caracteres Unicode.&#x20;
* Representa cualquier mensaje ASCII sin cambios: compatible con versiones anteriores del ASCII.&#x20;
* Ahorra espacio de almacenamiento para textos en caracteres latinos.

## Representación de imágenes

Se representan de dos maneras:&#x20;

* Gráfico de mapas de bits
* Gráfico de vectores

### Mapa de bits

Una imagen se divide en una matriz de pixeles.&#x20;

A cada pixel se le asigna un patrón de bits. El tamaño y el valor del patrón depende de la imagen, para una imagen formada solo por puntos blancos y negros, un patrón de un bit es suficiente para representar un pixel.

<figure><img src="../.gitbook/assets/image (339).png" alt="" width="375"><figcaption></figcaption></figure>

Para representar imágenes a color, cada pixel de color se descompone en tres colores primarios: <mark style="color:red;">rojo</mark>, <mark style="color:green;">verde</mark>, <mark style="color:blue;">azul</mark> (RGB).&#x20;

Cada pixel tiene tres patrones de bits:&#x20;

* uno para representar la intensidad del color rojo,&#x20;
* otro para la intensidad del color verde
* el tercero para la intensidad del color azul

### Gráfico de vectores

No guarda los patrones de bits. La imagen se descompone en una combinación de curvas y líneas. Cada curva o línea se representa por medio de una formula matemática. En este caso cada vez que se dibuja la imagen, la formula se vuelve a evaluar.

## BASE 64

Es un sistema de numeración posicional de base 64. Es la mayor potencia que podemos representar usando solo los caracteres ASCII. Se usa en codificaciones de E-mail y PGP entre otras aplicaciones.  Por tanto, es un método de codificación que convierte datos binarios en una representación de texto ASCII utilizando un conjunto de 64 caracteres.

El alfabeto consta de 64 caracteres \[A-Z]\[a-z]\[0-9] y los símbolos / y +

Esto es: <mark style="color:red;">ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/</mark>

&#x20;Su funcionamiento es como sigue:

1. Los datos binarios se dividen en bloques de 3 bytes, o sea 24 bits. Si al final quedan menos de 3 bytes para codificar, se agregan bytes nulos para formar un bloque completo.
2. Cada bloque de 3 bytes se convierte en un número entero de 24 bits, que es equivalente a 3 bytes en binario.
3. El número entero de 24 bits se divide en 4 números enteros de 6 bits.
4. Cada número entero de 6 bits se mapea a un carácter del conjunto de caracteres Base64. Estos caracteres están definidos como: `A-Z`, `a-z`, `0-9`, `+`, `/`.
5. Si el número de bytes de entrada no es divisible por 3, se añaden uno o dos caracteres de relleno (=) al final de la cadena Base64 para asegurar que la longitud total sea un múltiplo de 4.

### **Ejemplo (1)**

Vamos a ver como se transforma la palabra `Hi!` codificada en Base64:

1. Convertir cada caracter a su correspondiente valor ASCII:
   * H -> 72
   * i -> 105
   * ! -> 33
2. Convertir esos valores en su correspondiente binario:
   * H -> 01001000
   * i -> 01101001
   * ! -> 00100001
3. Combinar estos bits en un solo bloque de 24 bits:\
   &#x20;`01001000 01101001 00100001`
4. Dividir este bloque en 4 bloques de 6 bits: \
   `010010` `000110` `100100` `100001`
5. Convertir cada bloque de 6 bits a sus valores decimales:
   * `010010` -> 18
   * `000110` -> 6
   * `100100` -> 36
   * `100001` -> 33
6. Mapear los valores decimales en caracteres Base64:
   * 18 -> S
   * 6 -> G
   * 36 -> k
   * 33 -> !

Por lo tanto, la palabra `Hi!` en Base64 representa como `SGk=`.

Para realizar la decodificación de Base64 simplemente realizamos el proceso inverso:&#x20;

* tomamos la cadena en Base64,&#x20;
* convertimos  cada carácter a su valor numérico de 6 bits,&#x20;
* combinamos estos valores para formar bloques de 24 bits, y&#x20;
* convertimos estos bloques de bits de vuelta a los datos binarios originales.

### **Ejemplo (2)**

Pensemos en este otro ejemplo. La palabra `Man`.

<figure><img src="../.gitbook/assets/image (340).png" alt=""><figcaption></figcaption></figure>

### **Ejemplo (3)**&#x20;

Tomemos la palabra `Bola`&#x20;

* Buscamos el binario de cada letra en ASCII&#x20;

<figure><img src="../.gitbook/assets/image (341).png" alt=""><figcaption></figcaption></figure>

* Dividimos en grupos de 6 y rellenamos con ceros a la derecha (4 en este caso)&#x20;

<figure><img src="../.gitbook/assets/image (342).png" alt=""><figcaption></figcaption></figure>

* Ahora tenemos que convertir de 6 bits a 8 bits, para lo cual agregamos 2 ceros (00) a la izquierda:&#x20;

<figure><img src="../.gitbook/assets/image (343).png" alt=""><figcaption></figcaption></figure>

* Volvemos a la tabla ASCII - buscamos los binarios correspondientes y tomamos nota de su número decimal:&#x20;

<figure><img src="../.gitbook/assets/image (344).png" alt=""><figcaption></figcaption></figure>

* Por último, buscamos la referencia decimal en la tabla Base64:

<figure><img src="../.gitbook/assets/image (345).png" alt=""><figcaption></figcaption></figure>

A continuación te dejo algunos casos comunes en los que utilizar Base64:

1. **Autenticación y cifrado**: Se puede utilizar para ciertas aplicaciones de autenticación o para la representación de datos cifrados en un formato legible.
2. **Intercambio de datos en la web**: Útil para incrustar imágenes en documentos HTML o CSS.
3. **Transmisión de datos binarios en texto**: En el correo electrónico, en HTTP, donde solo se permiten caracteres de texto legibles. Al codificar datos binarios en Base64, estos datos pueden enviarse como texto ASCII.
4. **Incorporación de datos binarios en formatos de texto**: Algunos formatos de archivo o protocolos, como XML o JSON, no admiten directamente la inclusión de datos binarios. Al codificar los datos binarios en Base64, pueden ser incluidos como una cadena de texto en estos formatos.
5. **Protección de datos binarios en sistemas que podrían alterarlos**: Algunos sistemas de transmisión o almacenamiento podrían alterar datos binarios si no se representan correctamente. Base64 proporciona una representación segura y confiable de datos binarios.

Base64 convierte datos binarios en un formato de texto legible y seguro, pero conlleva un aumento del tamaño de los datos (aproximadamente un 33% de aumento), debido a la forma en que los bits se agrupan en bloques de 6 bits para representar los 64 caracteres.

## HEX

Es un sistema de numeración posicional basado en 16 dígitos del 0 al 9 (0, 1, 2, 3, 4, 5, 6, 7, 8, 9) las letras de la A a la F (A, B, C, D, E, F).&#x20;

* Se utiliza para registrar valores numéricos en los registros de memoria.&#x20;
* Ocupa menor cantidad de dígitos a la hora de almacenar datos y valores numéricos muy grandes.&#x20;
* Cada dígito hexadecimal se representa con 4 dígitos binarios.&#x20;
* Facilita la conversión y almacenamiento de números en los dispositivos electrónicos, memorias y equipos.
* Es una notación más compacta y utiliza menos dígitos que el sistema binario.

<figure><img src="../.gitbook/assets/image (346).png" alt=""><figcaption><p>Representación en HEX</p></figcaption></figure>

##

## Links

* https://programmerclick.com/article/9056135008/ &#x20;
* https://marquesfernandes.com/es/tecnologia-es/que-y-base64-para-que-serve-y-como-funciona/ https://gchq.github.io/CyberChef/#recipe=To\_Base64('A-Za-z0-9%2B/%3D')\&input=Qm9sYQ  &#x20;
* https://gchq.github.io/CyberChef/#recipe=To\_Base64('A-Za-z0-9%2B/%3D')\&input=TWFu&#x20;

