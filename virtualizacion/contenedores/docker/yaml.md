# YAML

**YAML** _ <mark style="color:blue;">`Ain't Markup Language`</mark>_ (YAML no es un lenguaje de marcado) fue propuesto por [Clark Evans ](https://clarkevans.com)en el 2001 y diseñado junto a Ingy `döt` Net y Oren Ben-Kiki.

Se trata de un **** lenguaje de declaración de datos **** que facilita la legibilidad y la capacidad de escritura del usuario. Este formato de <mark style="color:blue;">`serialización`</mark> se encarga de almacenar archivos de configuración y se puede usar junto con todos los lenguajes de programación.&#x20;

Está inspirado en lenguajes del tipo XML, C, Python y Perl, así como en el formato de los [correos electrónicos](https://es.wikipedia.org/wiki/Correo\_electr%C3%B3nico).

En sus inicios, YAML quería decir _<mark style="color:blue;">`Yet Another Markup Language`</mark>_ (otro lenguaje de marcado más) para distinguir su propósito centrado en los datos del marcado de documentos. Sin embargo, es <mark style="color:blue;">`XML`</mark> el que se usa frecuentemente para serializar datos y éste es un auténtico lenguaje de marcado de documentos, con lo cual, es razonable considerar YAML como un lenguaje de marcado ligero.

Hemos dicho que YAML es un formato de serialización pero ¿Qué es esto? pues se trata de un proceso de codificación de un objeto (hablando de programación orientada a objetos) en un medio de almacenamiento que puede ser un archivo o buffer de memoria para poder transmitirlo a través de una conexión en red en un formato más legible, como son [XML](https://es.wikipedia.org/wiki/Extensible\_Markup\_Language) o [JSON](https://es.wikipedia.org/wiki/JSON).&#x20;

En resumen, la <mark style="color:blue;">`serialización`</mark> es un mecanismo para:&#x20;

* transportar objetos a través de la red,&#x20;
* hacer persistente un objeto en un archivo o base de datos
* &#x20;distribuir objetos idénticos a varias aplicaciones o localizaciones.

Entre las características o ventajas de un archivo YAML tenemos:

* permite al usuario tener una **mejor lectura de los datos** y&#x20;
* facilita la escritura de los datos, permitiendo crear estructuras más complejas que las que se pueden hacer usando una línea de comandos.
* **evita que el usuario tenga que especificar todos los parámetros en la línea de comandos.**&#x20;
* presenta facilidad respecto a su **mantenimiento**, debido a que se puede administrar con un sistema de gestión de versiones, donde se registran los cambios y modificaciones realizadas.
* **admite un numeroso grupo de tipos de datos**, como diccionarios, matrices, listas y escalares.
* es compatible con lenguajes de programación como JavaScript, Python, Java o Rubí, entre otros.

### Sintaxis

Relativamente sencilla ya que fue diseñada teniendo en cuenta que fuera muy legible pero que a la vez fuese mapeable a los tipos de datos más comunes en la mayoría de los lenguajes de alto nivel.&#x20;

La sintaxis básica de este tipo de ficheros indica que cada archivo YAML inicia con <mark style="color:blue;">****</mark><mark style="color:blue;">** **</mark>_<mark style="color:blue;">**`---`**</mark>_ <mark style="color:blue;"></mark><mark style="color:blue;"></mark> y su extensión es _<mark style="color:blue;">`.yaml`</mark>_.

La estructura es _<mark style="color:blue;">**`clave: valor`**</mark><mark style="color:blue;">**  **</mark><mark style="color:blue;">****</mark>_  resultando fundamental dejar un espacio entre los indicadores.

Se permiten datos como:  **caracteres, cadenas, valores flotantes, números enteros,** así como colecciones (por ejemplo matrices) y listas construidas partiendo de otros datos básicos.

YAML **solo admite espacios** y tiene la capacidad de distinguir entre mayúsculas y minúsculas.

Utiliza una notación basada en la sangría y/o un conjunto de caracteres <mark style="color:blue;">`Sigil`</mark> distintos de los que se usan en XML, haciendo que sea fácil componer ambos lenguajes.

* Los contenidos se describen utilizando el conjunto de caracteres imprimibles de Unicode, bien en UTF-8 o UTF-16.
* En la estructura del documento se indenta utilizando espacios en blanco, aunque  no se permite el uso de caracteres de tabulación.

****

****

### Links

* [https://yaml.org ](https://yaml.org)
*
