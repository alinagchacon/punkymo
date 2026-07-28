# Headscale

**Headscale** es la alternativa libre y **self-hosted** al Control Plane oficial. Es un proyecto de código abierto que implementa las mismas API, permitiéndote:

1. Mantener la soberanía total sobre tus metadatos y la gestión de la red.
2. Utilizar los **clientes oficiales de Tailscale** simplemente apuntándolos a tu propia instancia.
3. Eliminar los límites de usuarios o dispositivos de los planes comerciales.

### **Instalación de Headscale**

Para que Headscale pueda coordinar las conexiones entre todos mis dispositivos, necesito alojarlo en una máquina que actúe como punto de encuentro permanente. A diferencia de los nodos de la VPN, que pueden entrar y salir de la red o cambiar de ubicación, el servidor de Headscale debe ser el ancla de la infraestructura.

_Nota: Ahora mismo, tengo headscale instalado en una VM de UBuntu Server, donde también tengo tailscale instalado._

#### **Características de la máquina**

El servidor donde instalé Headscale (normalmente un VPS o un servidor dedicado) debe cumplir con tres condiciones fundamentales para garantizar la estabilidad de la red (pero en mi caso, mi MV, no es así):

* **Accesibilidad Global:** La máquina debe tener una **IP pública estática** o un nombre de dominio (FQDN) asociado. Esto permite que cualquier nodo, esté donde esté, pueda localizar el Control Plane para solicitar instrucciones de conexión.
* **Seguridad mediante HTTPS:** Es obligatorio que el tráfico entre los clientes y Headscale viaje cifrado. Necesitarás un certificado SSL (puedes usar Let’s Encrypt) para que la comunicación se realice de forma segura a través del puerto **443**.
* **Disponibilidad (Uptime):** Si el servidor de Headscale se apaga, los nodos nuevos no podrán unirse y los existentes no podrán actualizar sus tablas de rutas si hay cambios en la red.

Nota: headscale está instalado en el VPS de Leo, por tanto, si que cumple con estos requisitos.

### **Registro de Nodos: conectando los dispositivos o nodos**

Una vez que el servidor está listo y el usuario creado, el siguiente paso es registrar los dispositivos (nodos). Mientras que **Headscale** opera exclusivamente en el **Control Plane** (gestionando la lógica de red y la distribución de llaves), la conectividad efectiva requiere el despliegue del cliente oficial de **Tailscale** en cada nodo. Este cliente actúa en el **Data Plane**, integrándose con el stack de red del sistema operativo a través de una interfaz virtual **TUN** y gestionando el cifrado y encapsulamiento de paquetes mediante **WireGuard**.

### Crear el contenedor

docker run -d \\\
\--name headscale-server \\\
-p 444:8080 \\\
-v /etc/headscale:/etc/headscale \\\
-v /var/lib/headscale:/var/lib/headscale \\\
headscale/headscale:0.29 \\\
headscale serve

sudo docker run -d \\\
\--name headscale \\\
\--restart unless-stopped \\\
-p 0.0.0.0:444:8080 \\\
-v /etc/headscale:/etc/headscale \\\
-v /var/lib/headscale:/var/lib/headscale \\\
headscale/headscale:0.29 \\\
serve

### Show help

docker exec -it headscale headscale help

### Show help for a specific command

docker exec -it headscale headscale \<COMMAND> --help

### **Create a headscale user**

docker exec -it headscale headscale users create \<USER>

### Headscale en el VPS de Leo

Con el servidor de control levantado y escuchando perfectamente en `[<https://kirby.e.taller404.org:444>]`, es el momento de expandir la topología de red.

En la arquitectura de esta tecnología, el VPS actúa únicamente como el **servidor de coordinación**, mientras que los dispositivos que se unen a la red privada virtual son los **nodos**.

"Ejecutar o usar" headscale desde un nodo puede significar dos cosas muy distintas en el diseño de una red. Vamos a ver cuál de ellas es tu objetivo actual:

**Dos caminos para interactuar desde un nodo**

* **Camino 1: Conectar el nodo a tu VPN (Tailnet)**\
  Consiste en instalar el cliente oficial de **Tailscale** en tu máquina local (tu PC, un móvil, otra VM) y configurarlo para que se registre en tu servidor de Headscale en lugar de los servidores oficiales de Tailscale.
* **Camino 2: Administrar Headscale en remoto desde tu máquina de trabajo**\
  Consiste en utilizar el ejecutable `headscale` en tu ordenador local para gestionar el servidor (crear usuarios, autorizar máquinas, ver el estado de la red) mediante llamadas de API seguras, evitando tener que hacer SSH al VPS cada vez que quieras hacer una gestión.

![image.png](attachment:29ef5667-e948-4faa-b54e-baff765c3219:image.png)

Lo lógico es el camino 1 pero se me ha complicado todo, así que continúo con el camino 2.

Desde el terminal:

export HEADSCALE\_CLI\_ADDRESS="[https://kirby.e.taller404.org:444](https://kirby.e.taller404.org:444)"\
export HEADSCALE\_CLI\_API\_KEY="TU\_API\_KEY\_AQUÍ"

### A tener en cuenta

*   gRPC

    ### **gRPC**[**¶**](https://headscale.net/stable/ref/api/#grpc)

    The gRPC interface can be used to control a Headscale instance from a remote machine with the `headscale` binary.

    #### **Prerequisite**[**¶**](https://headscale.net/stable/ref/api/#prerequisite)

    * A workstation to run `headscale` (any supported platform, e.g. Linux).
    * A Headscale server with gRPC enabled.
    * Connections to the gRPC port (default: `50443`) are allowed.
    * Remote access requires an encrypted connection via TLS.
    * An [API key](https://headscale.net/stable/ref/api/#api) to authenticate with the Headscale server.

    #### **Setup remote control**[**¶**](https://headscale.net/stable/ref/api/#setup-remote-control)

    1. Download the [`headscale` binary from GitHub's release page](https://github.com/juanfont/headscale/releases). Make sure to use the same version as on the server.
    2. Put the binary somewhere in your `PATH`, e.g. `/usr/local/bin/headscale`
    3. Make `headscale` executable: `chmod +x /usr/local/bin/headscale`
    4. [Create an API key](https://headscale.net/stable/ref/api/#api) on the Headscale server.
    5. Provide the connection parameters for the remote Headscale server either via a minimal YAML configuration file or via environment variables:
*   [DERP (Designated Encrypted Relay for Packets)](https://tailscale.com/docs/reference/derp-servers) server

    A [DERP (Designated Encrypted Relay for Packets) server](https://tailscale.com/docs/reference/derp-servers) is mainly used to relay traffic between two nodes in case a direct connection can't be established. Headscale provides an embedded DERP server to ensure seamless connectivity between nodes.

    ### **Configuration**[**¶**](https://headscale.net/stable/ref/derp/#configuration)

    DERP related settings are configured within the `derp` section of the [configuration file](https://headscale.net/stable/ref/configuration/). The following sections only use a few of the available settings, check the [example configuration](https://headscale.net/stable/ref/configuration/) for all available configuration options.

    #### **Enable embedded DERP**[**¶**](https://headscale.net/stable/ref/derp/#enable-embedded-derp)

    Headscale ships with an embedded DERP server which allows to run your own self-hosted DERP server easily. The embedded DERP server is disabled by default and needs to be enabled. In addition, you should configure the public IPv4 and public IPv6 address of your Headscale server for improved connection stability:

    **config.yaml**

    ```
    derp:server:enabled:trueipv4:198.51.100.1ipv6:2001:db8::1
    ```

    Keep in mind that [additional ports are needed to run a DERP server](https://headscale.net/stable/setup/requirements/#ports-in-use). Besides relaying traffic, it also uses STUN (udp/3478) to help clients discover their public IP addresses and perform NAT traversal. [Check DERP server connectivity](https://headscale.net/stable/ref/derp/#check-derp-server-connectivity) to see if everything works.

### Link

* [https://www.josedomingo.org/pledin/2026/02/vpn-mesh-headscale/](https://www.josedomingo.org/pledin/2026/02/vpn-mesh-headscale/)
* Instalado en docker:
  * [https://headscale.net/stable/ref/configuration/](https://headscale.net/stable/ref/configuration/)
  * [https://headscale.net/stable/usage/getting-started/#\_\_tabbed\_1\_2](https://headscale.net/stable/usage/getting-started/#__tabbed_1_2)
* [https://blog.taller404.org/posts/arcane/](https://blog.taller404.org/posts/arcane/) - blog de Leo
* [https://getarcane.app/docs](https://getarcane.app/docs)

NO LO PUDE SOLUCIONAR
