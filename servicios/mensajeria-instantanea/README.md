# Mensajería instantánea

A diferencia del correo electrónico, que es comunicación asíncrona, la mensajería instantánea requiere `presencia` e `inmediatez`.

La mensajería instantánea - M.I. es un servicio diseñado para facilitar conversaciones en línea en tiempo real entre dos o más personas.

A diferencia de otras formas de comunicación digital, permite el intercambio inmediato de texto, archivos y datos.

Aunque sus orígenes se remontan a los sistemas de comunidades autónomas conocidos como los [BBS (Bulletin Board System)](https://www.neoteo.com/la-historia-de-los-bulletin-board-system-bbs), la mensajería instantánea moderna se popularizó tras el lanzamiento de software como [ICQ ](https://es.wikipedia.org/wiki/ICQ)en 1996.

### Algunas características de la M.I.

Los sistemas de mensajería tienen unas funciones básicas comunes a todos:

* **Estado de presencia**: Permite saber si el usuario está conectado, ausente o no disponible.
* **Baja latencia**: La entrega debe ser prácticamente en tiempo real.
* **Multimedia**: Ya no solo es texto, también se puede transmitir voz, video y archivos.

### Arquitectura de los Sistemas de Mensajería

En redes, la mayoría de los servicios de IM profesionales siguen un modelo `Cliente-Servidor`, aunque existen alternativas.

* **Cliente**: Es la aplicación o interfaz instalada en el dispositivo del usuario final.
* **Servidor**: Es el núcleo gestionado por el proveedor del servicio y se encarga de la autenticación de los usuarios, la gestión de permisos y el enrutamiento de los mensajes. En este modelo, los usuarios no se conectan directamente entre sí, sino que el servidor actúa como el nodo central que coordina toda la comunicación.

Adicionalmente tenemos dos modelos:

* **Propietario**: El servidor es como una **caja negra** controlada por una empresa como puede ser WhatsApp, Telegram, Slack, etc. Los cliente se conectan a su nube.
* **Abierto**: Similar al correo electrónico. Yo puedo tener mi servidor en la empresa A y hablar con alguien de la empresa B. El protocolo rey aquí es <mark style="color:purple;">XMPP</mark>.

### Principales protocolos

El funcionamiento de la M.I. depende de protocolos específicos de comunicación, donde los más destacados son:

* **XMPP - Extensible Messaging and Presence Protocol**: Es un estándar abierto basado en el lenguaje [XML](https://es.wikipedia.org/wiki/Extensible_Markup_Language). Su comunicación se organiza mediante instancias de tres tipos:
  * **Message**: Para los mensajes de chat.
  * **Presence**: Para notificar si un usuario está disponible, ocupado u offline.
  * **IQ - Information Query**: Para realizar solicitudes y respuestas de información técnica por debajo de la interfaz.
* **WebSockets**: <mark style="color:purple;">Es una tecnología fundamental para el chat en la web moderna</mark>, ya que proporciona un <mark style="color:purple;">canal bidireccional y full-duplex</mark> sobre una única conexión TCP persistente. Esto permite que tanto el cliente como el servidor envíen mensajes de forma simultánea con una latencia muy baja.
* **SSE - Server-Sent Events**: A diferencia de WebSockets, <mark style="color:purple;">SSE es unidireccional</mark>; el servidor mantiene una conexión HTTP abierta para enviar flujos de datos de forma continua al cliente. Es ideal para notificaciones o chats ligeros donde el cliente no necesita enviar datos constantemente.
* **SIP - Session Initiation Protocol**: Utilizado frecuentemente en entornos corporativos para enviar mensajes y gestionar sesiones dentro de redes privadas.

### Jabber - el protocolo estándar XMPP

Si hay un protocolo que debemos conocer en este entorno es el [XMPP (Extensible Messaging and Presence Protocol)](https://xmpp.org/es/), que es un estándar abierto basado en XML.

#### ¿Qué es y cómo funciona?

Jabber es un protocolo abierto basado en el estándar XML para el intercambio en tiempo real de mensajes entre dos usuarios en Internet.

La principal aplicación de Jabber es una plataforma de mensajería y una red de mensajería instantánea que ofrece una funcionalidad similar a la de otros sistemas del tipo: AIM, ICQ, MSN Messenger y Yahoo.

Por otra parte, Jabber permite enviar mensajes a usuarios que no están conectados, permite conectar tu cuenta desde varios sitios al mismo tiempo, conectar a otras redes como MSN, AIM o Yahoo!, etc.

> _<mark style="color:purple;">Los servidores Jabber de todo el mundo conforman una federación de mensajería instantánea en la que todo el mundo puede hablar entre sí sin restricción alguna.</mark>_<br>

Una característica de jabber es que es seguro puesto que cualquier servidor de Jabber puede ser aislado de la red pública. Ademá ,cualquier implementación del servidor usa SSL para las comunicaciones cliente-servidor y numerosos clientes soportan PGP-GPG para encriptar las comunicaciones de cliente a cliente.

En resumen, Jabber está basado en el protocolo XMPP, que es un protocolo extensible, abierto y estándar basado en XML para el intercambio en tiempo real de mensajes y presencia entre dos puntos en Internet.

<figure><img src="../../.gitbook/assets/image (8).png" alt="" width="563"><figcaption><p>Tomado de: <a href="https://www.jabberes.org/jabber/introduccion/">https://www.jabberes.org/jabber/introduccion/</a></p></figcaption></figure>

En Jabber la dirección de cada usuario depende del servidor en el que tenga la cuenta. La sintaxis que sigue es: <mark style="color:purple;">nombre\_de\_usuario@nombre\_de\_servidor.</mark> Esto es,

1. **Direccionamiento**: Usa el llamado JID (Jabber ID), que tiene el formato usuario@dominio/recurso Por ejemplo: <mark style="color:purple;">pepe@informatica.es/movil</mark>
2. **Instancias**: Son los fragmentos de XML que se intercambian:
   1. \<presence>: Indica si estás online.
   2. \<message>: El contenido del chat.
   3. \<iq> (Info/Query): Para peticiones de configuración o datos del servidor.
3. **Dato técnico**: XMPP mantiene una conexión TCP persistente. Para ahorrar batería en móviles, hoy en día se suelen usar notificaciones Push (Firebase/APNs) para "despertar" al cliente.

### Otros protocolos y tecnologías relevantes

Aunque XMPP es el “académico”, en el mundo laboral podemos ver otros como:

* **MQTT**: Aunque es para IoT, se usa mucho en mensajería móvil (como Facebook Messenger) por su bajísimo consumo de ancho de banda.
* **Matrix**: El sucesor moderno de XMPP. Es un protocolo descentralizado que usa JSON y HTTP, muy popular actualmente por su seguridad y puentes con otras redes.
* **WebRTC**: Fundamental si vuestro servicio de mensajería va a incluir videollamadas directamente en el navegador sin plugins.

#### Características y funcionalidades clave

Para que la experiencia sea considerada "instantánea", los protocolos deben gestionar funciones críticas como:

1. **Gestión de Presencia**: Permite saber en todo momento si un contacto de la lista está conectado o disponible.
2. **Indicadores de Escritura**: Permiten visualizar en tiempo real cuándo el interlocutor está redactando una respuesta.
3. **Transferencia de Archivos**: La capacidad de enviar imágenes, sonidos o documentos directamente a través de la sesión de chat.
4. **Back Channel**: Una tendencia moderna donde los usuarios mantienen chats paralelos o hilos de conversación durante eventos en vivo para interpretar o comentar lo que sucede.

### Seguridad en la mensajería instantánea

Como administradores de red, la seguridad es nuestra prioridad. Debemos diferenciar dos conceptos:

1. **Cifrado en tránsito (TLS/SSL)**: El mensaje va seguro del cliente al servidor. El administrador del servidor podría leerlo.
2. **Cifrado de extremo a extremo (E2EE)**: Solo el emisor y el receptor tienen las llaves. Protocolos como Signal Protocol son el estándar de oro aquí.

### En resumen

Para poner en práctica un servidor de mensajería instantánea, podemos trabajar con soluciones que se puedan desplegar en servidores Linux:

| Servidor | Protocolo | Características                                                     |
| -------- | --------- | ------------------------------------------------------------------- |
| Ejabberd | XMPP      | El más robusto, escrito en Erlang. Muy escalable.                   |
| Openfire | XMPP      | Muy fácil de configurar (interfaz web, Java). Ideal para iniciarse. |
| Prosody  | XMPP      | Muy ligero, ideal para Raspberry Pi o servidores pequeños.          |
| Synapse  | Matrix    | El servidor de referencia para la red Matrix.                       |

#### Finalmente

Hoy en día, la mensajería instantánea ha evolucionado de ser una herramienta de entretenimiento doméstico a un componente vital en las infraestructuras de comunicación empresarial, integrándose incluso con inteligencia artificial para la atención al cliente en tiempo real.

\
Por tanto, para que un servicio de mensajería funcione necesitamos:

1. Un servidor que gestione las cuentas y la presencia.
2. Un protocolo común (XMPP, Matrix).
3. Un cliente (Pidgin, Gajim, Element).
4. Un puerto abierto en el firewall (típicamente 5222 para clientes XMPP).

### Links

* [https://xmpp.org/es/](https://xmpp.org/es/)
* [https://www.jabberes.org/jabber/introduccion/](https://www.jabberes.org/jabber/introduccion/)\
  <br>
