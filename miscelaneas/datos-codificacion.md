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

* ACKnowledge: acuse de recibo -  mensaje que el destino envía al origen para confirmar la recepción de un mensaje.&#x20;
* Se definen también diferentes ACK con información más compleja como peticiones de reenvío de determinadas tramas o información sobre incidencias entre otros.&#x20;
* Los mensajes ACK se utilizan en la mayoría de las capas del OSI pero son esenciales en la capa 2 y 3.
* NACK (negative acknowledgement o acuse de recibo negativo – se envía para informar que en la recepción de una trama de datos ha habido un error.&#x20;
* Es el contrario de  ACK.&#x20;
* SYN – sincronía en espera dado que no hay bits de start, stop o paridad presentes en los sistemas de comunicación en serie síncronos, hubo que establecer una serie de caracteres para el reconocimiento de los caracteres a sincronizar en el envío de información.



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

| Mapas de bits                                                                                                                                                                                                                                                                                      | Gráfico de vectores                                                                                                                                                                                                                                |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Una imagen se divide en una matriz de pixeles. </p><p>A cada pixel se le asigna un patrón de bits. El tamaño y el valor del patrón depende de la imagen, para una imagen formada solo por puntos blancos y negros, un patrón de un bit es suficiente para representar un pixel.</p>             | No guarda los patrones de bits. La imagen se descompone en una combinación de curvas y líneas. Cada curva o línea se representa por medio de una formula matemática. En este caso cada vez que se dibuja la imagen, la formula se vuelve a evaluar |
| <img src="../.gitbook/assets/image (2).png" alt="" data-size="original">                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| Para representar imágenes a color, cada pixel de color se descompone en tres colores primarios: rojo, verde, azul (RGB). Cada pixel tiene tres patrones de bits: uno para representar la intensidad del color rojo, uno para la intensidad del color verde y uno para la intensidad del color azul |                                                                                                                                                                                                                                                    |

## BASE 64

Es un sistema de numeración posicional de base 64. Es la mayor potencia que podemos representar usando solo los caracteres ASCII. Se usa en codificaciones de E-mail y PGP entre otras aplicaciones. El alfabeto consta de 64 caracteres \[A-Z]\[a-z]\[0-9] y los símbolos / y +

Esto es: ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/



Los equipos solo “entienden” el sistema binario (0 y 1), con lo que para enviar datos como texto o imágenes primero se debe pasar a binario, enviar y nuevamente decodificar.&#x20;

Sin embargo, la conversión no funciona de manera directa porque por la red solo se transmiten caracteres “imprimibles”.&#x20;

¿Qué son los caracteres imprimibles? Volvamos a ASCII.

Los primeros 32 caracteres son de “control” y van del 0 a 31 Los siguientes 95 caracteres del 32 al 128 caracteres son imprimibles, con lo cual por la red solo se pueden transmitir estos 95 caracteres.

Como hay sistemas que solo pueden usar los caracteres ASCII, Base64 resulta ser un método utilizado para convertir datos de caracteres no ASCII a ASCII. Los caracteres ASCII usan un patrón de 7 bits. Sin embargo, la mayoría de los equipos almacenan en bytes (1 byte = 8 bits) Por lo que ASCII deja de ser adecuado y viene Base64 a solucionar el problema.

