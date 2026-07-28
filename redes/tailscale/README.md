# Tailscale

## Introducción

Tailscale es una red privada virtual (VPN) de tipo mesh o malla basada en el protocolo WireGuard.

La capa base es precisamente este protocolo de código abierto WireGuard, y más específicamente la variante _wireguard-go_, que se ejecuta en el espacio de usuario. WireGuard crea un conjunto de túneles cifrados y ligeros entre nuestro equipo, máquina virtual o contenedor (punto final o nodo) y cualquier otro nodo de la red.

La mayoría de las veces, los usuarios de VPN —incluidos los de WireGuard— implementan una arquitectura de tipo «estrella» llamada también _hub-and-spoke_, en la que cada dispositivo cliente se conecta a un switch central o puerta de enlace VPN.

Sin embargo, a diferencia de las VPN tradicionales como OpenVPN o IPSec, Tailscale permite que **los dispositivos se conecten directamente entre sí (punto a punto)**, creando una red local virtual segura sin importar en qué parte del mundo estén ni detrás de qué firewall se escondan.

### Topología de estrella

Una topología en estrella sería la forma más sencilla de configurar WireGuard: cada nodo de la red necesita conocer la clave pública, la dirección IP pública y el número de puerto de los demás nodos a los que desea conectarse directamente. En este tipo de topología, solo existe un nodo central (_hub_) y los nodos periféricos son conectados mediante «radios» o _spokes_, lo que lo hace mucho más simple.

![image.png](attachment:a6d91d8f-af9b-4fb5-b200-4e62e20fe02a:image.png)

El nodo central tiene una dirección IP estática y una regla de apertura en su firewall para que sea fácil localizarlo. Así, puede aceptar conexiones entrantes de nodos con otras direcciones IP —incluso si estos se encuentran tras un firewall—, siguiendo el funcionamiento habitual de los protocolos de Internet cliente-servidor.

La topologia de estrella funciona bien, pero no sin presentar algunos inconvenientes:

* En una configuración de VPN tradicional, las empresas instalan un único concentrador VPN y establecen túneles secundarios (mediante IPsec) entre las distintas ubicaciones. De este modo, los usuarios se conectan a la VPN en una ubicación y su tráfico se reenvía a su destino final en otra distinta. Sin embargo, la mayoría de las empresas modernas no tienen un único lugar que deseen designar como nodo central. Por el contrario, cuentan con múltiples oficinas, diversos centros de datos en la nube, regiones o VPC, entre otros elementos.

Las configuraciones tradicionales pueden resultar difíciles de escalar: los usuarios remotos pueden estar cerca o lejos del concentrador VPN; si están lejos, experimentan una latencia elevada al conectarse a él. También es posible que el centro de datos al que desean acceder tampoco esté cerca del concentrador VPN; si la distancia es grande, la latencia vuelve a ser elevada. Imagina que un empleado en Barcelona intenta acceder a un servidor también situado en Barcelona a través de la VPN de la sede central de la empresa que se encuentra en Madrid:

Sin embargo, WireGuard es diferente porque crea un conjunto de túneles que son lo suficientemente ligeros como para permitirnos crear una configuración de múltiples concentradores (multi-hub) sin demasiadas complicaciones:

Como nada es perfecto, hay un inconveniente y es que ahora cada uno de los centros de datos requiere una dirección IP estática, un puerto abierto en el firewall y un conjunto de claves de WireGuard. Al añadir un nuevo usuario, se debe distribuir la nueva clave a los otros servidores. Cuando se añade un nuevo servidor, debemos distribuir su clave a todos los usuarios.

Así funcionan las redes de tipo estrella (_hub-and-spoke_). No son demasiado difíciles de configurar con WireGuard, aunque sí tediosas, y todavía no hemos abordado las prácticas seguras para la gestión de claves.

Aun así, hay un punto incómodo en este modelo y es que no permite que los nodos individuales se comuniquen directamente entre sí.

Cuando se conectan directamente todos los nodos entre sí, se le llama red de malla (_mesh network_):

![image.png](attachment:cdd77131-b934-4667-be5d-bfcc87696928:image.png)

Esto facilitaría el diseño de aplicaciones _peer-to-peer_ pero resultaría complicado, puesto que, una red de 10 nodos necesitaría de 10 × 9 = 90 configuraciones de puntos finales de túnel WireGuard. Por otra parte, cada nodo necesita conocer su propia clave y otras 9 más, y habría que actualizar cada nodo siempre que se rotara una clave o se añadiera o eliminara un usuario.

Por si fuera poco, se necesitaría abrir un puerto en el firewall para cada nodo a fin de permitir conexiones entrantes en lugares como cafeterías, hoteles o aeropuertos. Todo ello sin contar con otros requisitos de seguridad que habría que considerar.

Aquí es donde entra Tailscale como solución a estos problemas.

### El plano de control

Ahora pensemos en crear una red en malla en el que todos los dispositivos estén conectados entre sí. Y como los firewallas y las direcciones IP dinámicas complican las cosas, nos olvidamos por ahora de su existencia, así que consideremos que todos usamos IP estáticas (IPv6).

¿De qué modo hacemos llegar todas las claves de cifrado de WireGuard (una versión simplificada y más segura de los certificados) a cada dispositivo?

Utilizando el software de código abierto Tailscale que se comunica con lo que se denomina servidor de coordinación que no sería otra cosa que un repositorio compartido para las claves públicas.

El llamado "plano de control" también hace de concentrador e intercambia claves de cifrado y establece políticas. El plano de datos es una malla. Funciona como un modelo híbrido centralizado-distribuido. La capacidad de procesamiento de datos de Tailscale aumenta con el número de nodos). Veamos:

* Cada nodo genera un par de claves pública/privada aleatorias y asocia la clave pública con su identidad (ver inicio de sesión, más abajo).
* El nodo se comunica con el servidor de coordinación y deja su clave pública, junto con una nota sobre su ubicación actual y el dominio al que pertenece.
* El nodo descarga una lista de claves públicas y direcciones de su dominio, que otros nodos han dejado en el servidor de coordinación.
* El nodo configura su instancia de WireGuard con el conjunto de claves públicas correspondiente.

No nos olvidemos que la clave privada nunca sale de su nodo dado que es lo único que podría usarse para suplantar la identidad de ese nodo al negociar una sesión de WireGuard. Por lo tanto, solo los dos nodos que se comunican entre sí pueden cifrar o descifrar paquetes para esa sesión de WireGuard. Es importante recordar que las conexiones de los nodos de Tailscale están cifradas de extremo a extremo (un concepto conocido como **red de confianza cero**.

### 2FA

Las claves públicas son precisamente eso —públicas—, por lo que no supone ningún riesgo que se filtren a terceros o incluso que se publiquen en un sitio web público. El funcionamiento es como el de un servidor SSH con el archivo `authorized_keys`: que contiene las claves públicas SSH y no es necesario mantenerlas en secreto:

Para gestionar la autenticación existen diferentes formas y una de ellas es implementar PSK que es un sistema de nombre de usuario y contraseña de claves precompartidas. Para configurar el nodo, basta con conectarse al servidor, introducir el nombre de usuario y la contraseña, y luego subir la clave pública propia y descargar las claves públicas publicadas por la cuenta del usuario o por otras cuentas del mismo dominio. Si se busca una solución más avanzada, es posible añadir autenticación de doble factor 2FA o MFA.

También se puede configurar el equipo con un certificado, o sea, una clave vinculada al dispositivo, en lugar de a la cuenta de usuario, que permitiría garantizar que un dispositivo no confiable nunca pueda publicar nuevas claves en el servidor de coordinación.

Tailscale opera un servidor de coordinación basado en estos conceptos aunque delega la gestión de los usuarios a un proveedor de servicios como: OAuth2, OIDC (OpenID Connect) o SAML. Entre los proveedores más utilizados se encuentran Gmail, GSuite y Office365.

![image.png](attachment:d72df816-086e-43ec-8690-493975987435:image.png)

El proveedor de identidad gestiona la lista de usuarios del dominio, las contraseñas, la configuración de autenticación de dos factores (2FA), etc. Todo esto elimina la necesidad de mantener un conjunto independiente de cuentas de usuario o certificados para la VPN, ya que se puede utilizar el sistema de autenticación que esté configurado para Google Docs, Office 365 u otras aplicaciones web. Como la información privada de las cuentas de usuario y los datos de inicio de sesión se alojan en otro servicio, Tailscale puede ofrecer un servicio de coordinación central fiable y almacenar la mínima información de identificación personal (PII) de los usuarios.

### NAT Traversal

Es poco probable que los nodos tengan una dirección IP estática y un puerto en el firewall abierto para el tráfico entrante de WireGuard. En la vida real, nos podemos encontrar que los nodos están en cafeterías, aviones o a redes LTE.

![image.png](attachment:6ebb40c5-1608-4383-979e-581924048624:image.png)

Eso implica dos capas de NAT y ningún puerto abierto. Tradicionalmente, se solía pedir que se habilitara UPnP (Universal Plug and Play) en el firewall, pero este protocolo de red permite a los dispositivos domésticos como consolas, ordenadores, etc. descubrirse y conectarse entre sí de modo automático, abriendo puertos en el router sin intervención manual.

Pero Tailscale emplea técnicas avanzadas basadas en los estándares de Internet como STUN e ICE para lograr las conexiones, eliminando la necesidad de configurar firewalls o de abrir puertos.

¿Qué es STUN?

STUN (_Session Traversal Utilities for NAT_) **es un protocolo de red que permite a los dispositivos detrás de un router o firewall conocer su dirección IP pública y el puerto asignado, facilitando** las comunicaciones directas en tiempo real (como VoIP o videollamadas) entre equipos.

Cuando los dispositivos se conectan a Internet desde una red doméstica o de oficina, utilizan un router que emplea **NAT** (_Network Address Translation_). La red local oculta la IP privada real del dispositivo asignándole una IP pública compartida. Si otro dispositivo externo intenta iniciar una llamada o conexión, no sabrá a dónde enviar los datos.

**La solución de STUN**

El dispositivo envía una consulta a un **servidor STUN** público. El servidor identifica desde qué IP pública y puerto exacto está llegando el tráfico a través del NAT. Con este dato, el dispositivo puede compartir esta información para establecer una conexión directa (_peer-to-peer_).

STUN es un componente fundamental para tecnologías de comunicación modernas que requieren flujos de audio y vídeo directos para ser rápidos y fluidos:

* **WebRTC:** permite hacer videollamadas desde el navegador sin instalar programas adicionales.
* **VoIP:** permite establecer llamadas telefónicas a través de Internet.
* **Aplicaciones de mensajería y videojuegos:** para mantener conexiones estables y de baja latencia.

¿Qué es ICE?

_Interactive Connectivity Establishment_ es una técnica y un marco de trabajo esencial utilizado para conectar directamente dos dispositivos a través de Internet, especialmente en aplicaciones de tiempo real como Voz sobre IP (VoIP), videollamadas (WebRTC) y mensajería.

Actúa como un investigador para encontrar la ruta de comunicación más directa y eficiente entre dos o más usuarios y lo consigue combinando dos protocolos estándar de la industria:

* **STUN (Session Traversal Utilities for NAT):** que permite que un dispositivo oculto detrás de un router NAT descubra su propia dirección IP pública y el puerto que su router le ha asignado.
* **TURN (Traversal Using Relays around NAT):** Cuando la conexión directa a través de STUN es imposible (debido a configuraciones de red muy estrictas), TURN entra como respaldo. Proporciona un servidor intermedio que reenvía los datos entre ambos dispositivos.

### ACL and security policies

Cuando pensamos en una VPN podemos caer en la idea de considerarlas un software de seguridad, dado que incorporan criptografía, claves públicas y privadas y certificados. Sin embargo, las VPN son softwares de conectividad dado que aumentan el número de dispositivos que pueden acceder a su red privada.

Lógicamente las VPN se combinan con los firewalls o se instalan desde un firewall qu es el dispositivo que realmente aplica los controles de acceso basados en direcciones IP y que permiten o entrar en la red.

Las reglas del firewall suelen basarse en direcciones IP y no en usuarios o roles, por lo que configurarlas de forma segura puede ser complicado y poco práctico. Por lo que se termina recurriendo a capas adicionales de autenticación, ya sea en la capa de transporte o en la de aplicación. ¿Por qué se necesitan protocolos como SSH o HTTPS? Porque la capa de red es demasiado insegura como para confiar en ella.

Por otra parte, los firewalls suelen estar dispersos por toda la organización y requieren una configuración individual. Si se dispone de una red con múltiples nodos centrales (por ejemplo, con distintos concentradores VPN en diferentes ubicaciones geográficas), es preciso asegurarse de configurar correctamente el firewall en cada uno de ellos para evitar brechas de seguridad.

Por último, configurar el dispositivo VPN o firewall de un fabricante específico para que autentique las conexiones VPN mediante el sistema de identidad de otro proveedor suele ser una tarea compleja. Es por ello que algunos proveedores de sistemas de identidad intentan venderle una VPN, mientras que algunos fabricantes de VPN tratan de venderle un sistema de identidad.

Cuando pasamos a una red de malla, no existe un punto central, por tanto las reglas del firewall se aplican en cada uno de los nodos, esto es, cada nodo bloquea las conexiones entrantes no autorizadas en el momento del descifrado.

Para facilitar este proceso, la política de seguridad de la empresa se almacena en el servidor de coordinación de Tailscale —centralizada en un único lugar— y se distribuye automáticamente a todos los nodos. De este modo, se mantiene un control centralizado de la política, pero con una aplicación eficiente y distribuida.

### Resumen

Como los dispositivos suelen estar detrás de routers con NAT (a veces NAT simétrico muy estricto), Tailscale utiliza técnicas de **STUN** (_Session Traversal Utilities for NAT_) para "perforar" los firewalls (UDP hole punching) y lograr que se conecten directamente. Si la conexión directa es absolutamente imposible debido a restricciones extremas de red, Tailscale enruta el tráfico cifrado a través de sus servidores de retransmisión llamados **DERP** (_Designated Encrypted Relay for Packets_) como último recurso.

**La red virtual (CGNAT):**\
Cada nodo recibe una dirección IP fija y única dentro del rango privado `100.64.0.0/10` (un rango reservado para CGNAT). No importa si tu móvil está en 5G y tu PC en la Wi-Fi de la universidad; para tu red privada (tu _Tailnet_), se verán como si estuvieran en el mismo conmutador virtual.

### Link

1. [https://tailscale.com/](https://tailscale.com/)
2. [https://github.com/juanfont/headscale/releases](https://github.com/juanfont/headscale/releases)
3. [Headscale](https://app.notion.com/p/Headscale-39d25955d56c80bab285c212bc2e9c03?pvs=21)
4. [Headscale propio + Proxy Inverso](https://app.notion.com/p/Headscale-propio-Proxy-Inverso-39e25955d56c80029947c57a178a2141?pvs=21)
