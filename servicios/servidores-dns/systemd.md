# Systemd

El `systemd` brinda resolución de nombres de red a las aplicaciones locales y el s`ystemd-resolved` es un servicio del `systemd`. Por tanto, el _`systemd-resolved`_ es una parte del paquete `systemd` que se instala por defecto.

Esta herramienta proporciona servicios de resolución para sistemas de nombres de dominio (DNS) e incluye al DNSSEC y el DNS sobre TLS), entre otros.

### Configuración

Podemos configurar la resolución de nombres editando el archivo /etc/systemd/resolved.conf y/o colocando archivos con extensión _.conf_ en /etc/systemd/resolved.conf.d/.&#x20;

_El systemd-resolved_ tiene 4 modos  para manipular la resolución de nombres de dominio. Dos de ellos son:

1. Utilizar el archivo `stub` del DNS de `systemd`:&#x20;
   1. En`/run/systemd/resolve/stub-resolv.conf` y contiene el código local `127.0.0.53` como el único servidor DNS así como una lista de dominios de búsqueda.
   2. Se aconseja redirijir el archivo `/etc/resolv.conf` al archivo local de resolución de DNS `/run/systemd/resolve/stub-resolv.conf` quees administrado por _systemd-resolved_.&#x20;
   3.  Se puede hacer con un enlace simbólico al archivo `stub` de systemd:

       ```
       # ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
       ```
2. Preservar _resolv.conf_:&#x20;
   1. De este modo se conserva el `/etc/resolv.conf`
   2. _El systemd-resolved_ es solo un cliente del archivo resolv.conf.&#x20;
   3. Modo menos disruptivo ya que `/etc/resolv.conf` puede continuar siendo administrado por otros paquetes.

