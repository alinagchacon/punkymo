# Seguridad en el correo

El servicio de correo electrónico brinda a las empresas enormes beneficios en cuanto a disponibilidad, accesibilidad, rapidez, posibilidad de enviar documentación a múltiples destinos, con acuse de recibo. Todo ello conlleva ahorro en tiempo y dinero. No en balde, ha sustituido casi por completo al correo tradicional e incluso a los servicios de mensajería.

Digamos que es un imprescindible a nivel empresarial sin embargo, es una de las fuentes más comunes de ataques y de malware por parte de los ciberdelincuentes. La tendencia natural de los usuarios es leer todos los correos y abrir los adjuntos que llegan, siendo el primer error de los muchos que podemos cometer cuando se trata del correo electrónico.

Simplificando un poco el proceso de enviar y recibir correos electrónicos, diríamos que una vez que  el emisor haya compuesto el mensaje para lo cual tiene que indicar los campos como: email de destino, asunto, cuerpo, la aplicación que hace de cliente de correo se encarga de enviarlo al servidor que tenga predeterminado (si hablamos de una empresa, sería el servidor corporativo de la misma).&#x20;

Una vez que el servidor recibe el correo, realiza una serie de comprobaciones:&#x20;

* si el contenido está correctamente formado
* si existe una dirección destino,&#x20;
* si el usuario está autorizado

En caso de que todo esté correcto, envía el mensaje al servidor destino, que lo procesa y almacena en el buzón del usuario al que va dirigido el correo para que éste pueda proceder a su lectura.&#x20;

Sin embargo, desde el punto de vista técnico y de la seguridad, se puede diferenciar dos partes en un correo electrónico: el <mark style="color:blue;">`cuerpo`</mark> y las <mark style="color:blue;">`cabeceras`</mark>.&#x20;

El <mark style="color:blue;">`cuerpo`</mark> del mensaje hace referencia al contenido en sí que, puede o no, incluir documentos adjuntos, y que depende por completo del usuario.&#x20;

Las <mark style="color:blue;">`cabeceras`</mark> contienen información facilitada por el usuario como el asunto, destinatario, emisor, y otra información añadida por el servidor o servidores por los que pasa el mensaje hasta llegar a su destino.

Precisamente son las cabeceras un detalle importante para determinar algunos aspectos de la seguridad de los correos. Las cabeceras nos permiten conocer el camino que un mensaje ha seguido entre emisor y receptor: cada vez que el correo pasa por un servidor de correo, éste añade un campo de datos a las cabeceras, con etiqueta <mark style="color:blue;">Received</mark>, donde especifica el nombre del servidor, su dirección IP, el servidor de correo utilizado, y la fecha y la hora en que se recibió el mensaje.&#x20;

De esta forma se puede determinar si un correo que viene de cierto dominio de Internet se ha procesado efectivamente en servidores relacionados con dicho dominio o por el contrario viene de sistemas que no son propios del mismo, indicando de este modo que el mensaje puede haber sido falsificado.

Teniendo en cuenta que el correo electrónico es una herramienta de comunicación imprescindible  para las empresas, es necesario definir su uso correcto y seguro. Piensa que, además de los abusos y errores no intencionados por parte de los mismos empleados que pueden causar perjuicio en la empresa, el correo electrónico es uno de los medios que utilizan los ciberdelincuentes para llevar a cabo sus ataques.&#x20;

Por error o no, los empleados pueden enviar documentos confidenciales a quien no deberían y desvelar, sin querer, la dirección del correo electrónico (que es un dato personal) de clientes o usuarios, o utilizar su correo corporativo para usos no permitidos.&#x20;

Es de todos conocidos la cantidad de correo SPAM que llega a los buzones, los correos de phishing que intentan robar credenciales o correos que suplantan entidades o personas. Para ello se basan en técnicas de ingeniería social que les permite conseguir sus fines (maliciosos) como puede ser robar credenciales o que les demos datos confidenciales.&#x20;

Entre las políticas a seguir en una empresa para el uso adecuado del servicio de correo electrónico podemos mencionar:

* El correo corporativo puede ser supervisado por la dirección de la empresa, incluyendo una cláusula en la normativa que firma el empleado.
* Instalación de aplicaciones antimalware
* Activación de los filtros antispam tanto en el servidor como en el cliente de correo según la Política Antimalware.
* Instalación de una tecnología de cifrado y firma digital para proteger la información confidencial. Por ejemplo, GPG, Mailvelope.
* Desactivar el formato HTML, la ejecución de macros y la descarga de imágenes
* Ofuscar las direcciones de correo electrónico. No se deben publicar las direcciones de correo corporativas en páginas web ni en redes sociales sin utilizar técnicas de ofuscación. Por ejemplo, utilizar una imagen con la dirección de correo.
* Uso de contraseñas seguras&#x20;
* Aprender a identificar correos sospechosos.
* Análisis de los documentos adjuntos.
* Evitar las redes públicas. Evitar utilizar el correo electrónico desde conexiones públicas, la wifi de una cafetería, el PC de un hotel, etc. Como alternativa, es preferible utilizar redes de telefonía móvil como el 3G o 4G.

### Links

* [https://www.incibe.es/protege-tu-empresa/blog/medidas-seguridad-correo-electronico](https://www.incibe.es/protege-tu-empresa/blog/medidas-seguridad-correo-electronico)
* [https://www.incibe.es/sites/default/files/contenidos/politicas/documentos/uso-correo-electronico.pdf](https://www.incibe.es/sites/default/files/contenidos/politicas/documentos/uso-correo-electronico.pdf)
