# Headscale propio + Proxy Inverso

Vamos a estructurar un archivo **`docker-compose.yml`** que levante de forma conjunta **Headscale** y un proxy inverso como **Caddy.** Me gusta la idea de Caddy por un alumno (Juan) que lo trabajó en su proyecto. Caddy genera los certificados HTTPS automáticos para un dominio local.

El uso de un proxy inverso (sea **Caddy** o cualquier otro) nos aporta dos ventajas:

1. **Seguridad perimetral**: dado que Caddy actúa como escudo, haciendo que todo el tráfico externo muera en Caddy, y este se comunica por la red interna de Docker con Headscale.
2. **Cifrado automático con Let's Encrypt / ZeroSSL**: Caddy gestiona, solicita y renueva los certificados SSL/TLS de forma 100% transparente sin que tengamos que configurar manualmente `certbot` ni scripts de renovación.



### Estructura del directorio

La estructura del directorio utilizado para recrear el docker compose de headscale y caddy es el siguiente:

<figure><img src="../../.gitbook/assets/image (926).png" alt="" width="375"><figcaption></figcaption></figure>

Como inicialmente tuve un montón de problemas, puse los directorios <mark style="color:violet;">`config`</mark> y <mark style="color:violet;">`headscale_data`</mark> con permisos para todos, esto es: chmod 777. No debe ser así.

El archivo `config/config.yaml` que requiere headscale es:

```jsx
# Configuración mínima recomendada para Headscale detrás de Proxy Inverso
server_url: <https://haven.local>
listen_addr: 0.0.0.0:8080
metrics_listen_addr: 127.0.0.1:9090

# Usamos SQLite para despliegues rápidos en laboratorio
database:
  type: sqlite3
  sqlite:
    path: /var/lib/headscale/db.sqlite

# Rango de IPs de asignación para la VPN (IPv4 y IPv6) (nuevo formato)
prefixes:
  v4: 100.64.0.0/10
  v6: fd7a:115c:a1e0::/48

derp:
  server:
    enabled: true
    region_id: 999
    region_code: "headscale-derp"
    region_name: "Headscale Embedded DERP"
    stun_listen_addr: "0.0.0.0:3478"
    private_key_path: /var/lib/headscale/derp_private.key
  urls:
    - <https://controlplane.tailscale.com/derpmap/default>

# Habilitar gRPC para cuando quieras administrarlo remotamente con la API Key
grpc_listen_addr: 0.0.0.0:50443
grpc_allow_insecure: true # Caddy se encarga de asegurar la conexión externa

noise:
  private_key_path: /var/lib/headscale/noise_private.key

# List of DNS servers to expose to clients.
# El server_url y base_domain no pueden usar el mismo dominio.
dns:
  override_local_dns: true
  magic_dns: true
  base_domain: vpn.haven.local  # <-- el dominio haven.local
  nameservers:
    global:                    # <-- Esta es la directiva global nueva
      - 1.1.1.1
      - 1.0.0.1
      - 2606:4700:4700::1111
      - 2606:4700:4700::1001

```

Archivo Caddyfile:

```jsx
# Configuración de Caddy para Headscale
# Reemplaza 'tu-dominio.duckdns.org' por tu dominio real (DDNS)

haven.local {
    # Redirige todo el tráfico inverso al contenedor de headscale en su puerto por defecto
    reverse_proxy headscale:8080 {
        # Cabeceras necesarias para el correcto funcionamiento de WebSockets y gRPC de Tailscale
        header_up Host {upstream_host_port}
        header_up X-Real-IP {remote_host}
        header_up X-Forwarded-For {remote_host}
        header_up X-Forwarded-Proto {scheme}
    }
}

```

Archivo docker-compose.yaml

```jsx
networks:
  headscale-net:
    driver: bridge

services:
  # --- PROXY INVERSO (CADDY) ---
  caddy:
    image: caddy:2-alpine
    container_name: caddy-proxy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "444:444"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./caddy_data:/data
      - ./caddy_config:/config
    networks:
      - headscale-net
    depends_on:
      - headscale

  # --- COORDINADOR VPN (HEADSCALE) ---
  headscale:
    image: headscale/headscale:latest
    container_name: headscale
    restart: unless-stopped
    volumes:
      - ./config:/etc/headscale
      - ./headscale_data:/var/lib/headscale
    entrypoint: headscale serve
    networks:
      - headscale-net

```

Levantar tailscale en el nodo:

```jsx
sudo tailscale up --login-server=https://haven.local --accept-dns=false
```

Para listar los nodos activos en headscale:

```jsx
sudo tailscale up --login-server=https://haven.local --accept-dns=false
```

Muestra lo siguiente:

<figure><img src="../../.gitbook/assets/image (928).png" alt=""><figcaption></figcaption></figure>

Para listar los usuarios habilitados:

```jsx
docker exec -it headscale headscale users list
```

Muestra algo como:

<figure><img src="../../.gitbook/assets/image (929).png" alt="" width="563"><figcaption></figcaption></figure>

### Resumen del proceso

#### Parte 1 - Añadir un nodo nuevo al servidor Headscale creado.

Como quiero añadir otro nodo al headscale para tenerlo claro de una vez, he creado una VM en VMWare (por la misma razón, para tenerlo más claro). Dicha VM está con Debian 13.6.

#### (1.1) Instalar el cliente de Tailscale

En la terminal de Debian 13.6, descargamos e instalamos el repositorio oficial:

```
curl -fsSL <https://tailscale.com/install.sh> | sh
```

#### (1.2) Inicio de sesión

Lanzamos el comando apuntando al controlador de control plane, que en mi caso es:`haven.local`:

```
sudo tailscale up --login-server <https://haven.local> --accept-dns=false
```

Esto me dará una nueva URL de registro con un token `hskey-authreq-...` único para esta máquina. **Copiamos la clave que nos  da.**

#### (1.3) Autorizo desde el servidor Headscale (Ubuntu Server)

Volvemos al terminal donde tengo instalado el servidor Headscale y ejecutamos el comando de registro asociándolo al usuario `kirby` que creamos antes:

```
docker exec -it headscale headscale nodes register --user kirby --key TU_NUEVA_CLAVE_DE_
```

_Nota: este comando con **register** está deprecated._

Yo tengo un usuario creado porque ya hice este proceso para añadir un nodo al servidor Headscale pero si no lo tuviera, tendría que hacer lo siguiente si quiero crear un usuario kirby:

```jsx
docker exec -it headscale headscale users create kirby
```

#### Parte 2 - Conectar un móvil Android

Por el aquello de utilizar **servicios IoT y BYOD (Bring Your Own Device)**. En los sistemas operativos móviles no tenemos una terminal para lanzar comandos, pero la app oficial de Tailscale permite cambiar el servidor de control.

#### (2.1) Instalar la App

Un paso elemental que ya tenía hecho porque me he conectado al servidor Headscale que me brindó Leo. En definitiva, para poder descargar la aplicación oficial de **Tailscale** nos vamos a la Google Play Store.

#### (2.2) Activar el "Modo Desarrollador" en la App para cambiar el servidor

Para indicarle a la aplicación que no use el servidor comercial de Tailscale y use tu `https://haven.local`, debemos hacer algo como lo siguiente:

1. Abrir la app de Tailscale en el móvil.
2. Ir al menú de opciones (los tres puntos en la esquina superior derecha o el menú de configuración).
3. Pulsar repetidamente (unas 5 o 10 veces) sobre el **logotipo de Tailscale** o sobre el nombre del usuario/versión hasta que se active el menú oculto de desarrollo.
4. Buscar la opción que dice **"Change server"** (o "Alternate server").
5. Introducir la URL del servidor, en mi caso: `https://haven.local` y guardar.

#### (2.3) Iniciar sesión y registrar

1. Pulsar en **Login / Connect**.
2. El navegador del móvil se abrirá intentando cargar la página de registro de Headscale. Algo así como: `https://haven.local/register/...`
3. Nos tenemos que asegurar que el móvil puede resolver el nombre `haven.local`  bien sea mediante DNS local o temporalmente abriendo la URL que nos dé el móvil directamente en el navegador del servidor.
4. El móvil nos mostrará en pantalla la clave de máquina (`hskey-authreq-...`).
5.  **Esa clave la registramos  en el servidor** usando el mismo comando de Docker:

    ```
    docker exec -it headscale headscale nodes register --user kirby --key CLAVE_DEL_MOVIL
    ```

#### Probando

Una vez que tengamos ambos dispositivos conectados, ejecutamos el comando para listar los nodos en el servidor Debian:

```
docker exec -it headscale headscale nodes list
```

Deberíamos ver tres nodos en la tabla con sus respectivas direcciones IPs `100.64.x.x`.

**Comprobando la conectividad:** Si intentamos hacer un `ping` desde la VM de Ubuntu hacia la IP `100.64.x.x` del móvil a través de la interfaz `tailscale`, ¿conseguimos establecerse la comunicación? ¿Qué latencias manejamos a través del túnel criptográfico?
