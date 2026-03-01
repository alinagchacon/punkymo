---
description: iptables
---

# Port Forward

### Escenario y características

Vamos a trabajar sobre el siguiente escenario en virtualbox:

* Dos VM donde una hace de <mark style="color:purple;">router</mark> con iptables y la otra es el <mark style="color:purple;">servidor web</mark> que nos mostrará una página web “cualquiera”.
  * **VM router**: ubuntu server
    * adaptador 1 - enp0s3: dhcp
    * adaptador 2 - enp0s8: IP estática con dhcp o definida en netplan.
  * **VM web**: ubuntu server
    * sólo está conectada a la red interna
    * instalar apache o nginx para tener una página web de prueba
    * puedes instalar también ssh para comprobar otros servicios

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

Podemos comprobar el funcionamiento de la página web utilizando el comando “curl”:

```bash
curl localhost
```

O bien nos aseguramos de tener el servicio activo:

```sh
sudo systemctl status nginx 
```

```bash
sudo systemctl status apache2
```

### ¿Y qué necesitamos hacer?

Ya tenemos listo el servidor en la red interna con la página web que queremos mostrar en el equipo anfitrión o host que se encuentra en la red externa, pública o outside. Así que vamos a configurar la VM que hace de router.

Pero ¿tenemos claro que queremos hacer? queremos poder visitar la página web que se encuentra en un servidor en la red interna, pero solo podemos “ver” la IP pública o externa del servidor que hace de router.

Veámoslo así: imagina que estás parado afuera del edificio donde vive un amigo al que fuiste a visitar. Desde de la calle, solo puedes ver la **puerta principal** del edificio. Supongamos que el edificio tiene un portero y para acceder al piso de tu amigo, le preguntas qué hacer. Finalmente el portero te envía directamente al piso de tu amigo.

* La puerta principal es el **router**.
* La **IP Pública** es la dirección del edificio.
* Los **Puertos** son los números de los pisos.

Esta idea, que se conoce como port forwarding o redireccionamiento de puertos, es una técnica de red que permite a dispositivos externos (desde Internet) conectarse a un dispositivo o servicio específico dentro de una red privada (LAN).

¿Quién es el **Port Forwarding** en nuestra imagen del edificio? Pues el portero de la finca que sabe que si alguien pregunta por el "piso 80", debe enviarlo directamente al salón del “segundo” piso.

### ¿Y técnicamente cómo funciona?

Un router actúa como una barrera de seguridad usando **NAT** (Network Address Translation). Los dispositivos internos pueden salir a Internet, pero nadie de fuera puede entrar a menos que el router sepa a dónde enviar ese tráfico.

Cuando configuras una regla de port forwarding, le dices al router:

> _"Cualquier paquete que llegue desde Internet al puerto A, envíalo a la dirección IP interna B en el puerto C"._

#### ¿En qué casos se usa?

Podemos utilizar esta técnica en casos como:

1. Tienes un **servidor web** o de archivos (NAS) en casa y quieres acceder a él desde el trabajo.
2. En **juegos online**, donde hay varios juegos que necesitan abrir puertos.
3. En **cámaras de seguridad,** para poder tener acceso a tus cámaras IP desde tu móvil cuando no estás en casa.
4. Para controlar tu PC desde otra ubicación, a través de un e**scritorio Remoto o RDP.**

#### ¿Y qué sucede con la seguridad?

Pues que abrir puertos es como dejar una ventana entreabierta en tu casa. Si el servicio que escucha en ese puerto (por ejemplo, un software de cámara antiguo) tiene vulnerabilidades, un atacante podría usar esa “ventana” para entrar en tu casa, vale decir, tu red privada.

Por eso, es recomendable:

* Abrir **solo** los puertos estrictamente necesarios.
* Usar puertos no estándar (por ejemplo, cambiar el 80 por el 8080) para evitar escaneos automáticos simples.
* Utilizar una **VPN** en lugar de **port forwarding** cuando sea posible, ya que es mucho más seguro.

### Configurando iptables

Ya sabemos que lo queremos hacer, entonces vamos a configurar iptables para que nos permita llegar al puerto 80 de la IP interna del servidor web, desde la IP pública o externa del equipo que hace de router.

#### **(1) Habilitando el ip\_forward**

Primero que nada, vamos a habilitar el IP forwarding y para ello nos vamos a editar el archivo `/etc/sysctl.conf` y quitar el # comentario en la línea: **`net.ipv4.ip_forward=1`**

```bash
nano /etc/sysctl.conf
```

Nos aseguramos que los cambios se hacen persistentes con:

```
sysctl -p
```

o bien:

```
systemctl restart systemd-sysctl.service
```

Si tecleamos el siguiente comando, nos debe mostrar el valor 0 o 1 para mostrar si tenemos activada o no el ip\_forward.

```
sysctl net.ipv4.ip_forward
```

#### **(2) Configurando iptables**

Recordemos que IPTABLES un **firewall** de Linux que permite **filtrar, permitir, bloquear y redirigir tráfico de red** a nivel del _kernel_, usando el subsistema **Netfilter**.

> _iptables decide qué paquetes de red entran, salen o atraviesan tu sistema._

Con iptables podemos:

* **Bloquear o Permitir tráfico** - firewall
* **Redirigir puertos -** port forwarding
* **Hacer NAT** - SNAT, DNAT, MASQUERADE
* **Controlar tráfico entre interfaces**
* Y más específicamente, crear escenarios de laboratorio (routers, firewalls, gateways) que es nuestro caso.

#### (2.1) ¿Tenemos iptables instalado?

Lo primero es saber si tenemos o no iptables instalado en nuestro sistema. Para ello podemos hacer:

```bash
whereis iptables
```

```bash
iptables —help
```

```bash
man iptables
```

#### (2.2) Eliminando reglas y creando las reglas “default”

Si lo tenemos instalado entonces podemos utiliza el comando siguiente para **listar (List)** todas las reglas que están configuradas actualmente en el firewall de Linux.

iptables -L

Si lo ejecutamos sin parámetros adicionales, nos muestra las reglas de la tabla **filter** que es la tabla por defecto y la que se encarga de decidir qué paquetes entran, salen o se reenvían.

***

Si hacemos:

iptables -t nat -L

Nos mostrará las reglas que gestionan el movimiento y la transformación de las direcciones IP y puertos, donde:

* **`t nat`**: indica que quieres consultar la **tabla NAT** (Network Address Translation).
* **`L`**: Es la instrucción para **Listar** las reglas.

#### **¿Y qué vemos en la salida?**

Vemos tres "cadenas" o chains:

1. **PREROUTING:** donde veremos la regla de **DNAT** del Port Forwarding que tenemos que hacer. Esto es el lugar donde el paquete "cambia de dirección" justo cuando entra al servidor, antes de que el sistema decida a dónde enviarlo.
2. **POSTROUTING:** Aquí veremos las reglas de **MASQUERADE** o **SNAT**. Es donde el paquete "se disfraza" con la IP del servidor antes de salir hacia el servidor interno (10.10.10.26).
3. **OUTPUT:** Reglas para paquetes generados por el propio servidor local que necesitan NAT.

Aunque no hayamos tocado nada, para asegurarnos que no hay reglas creadas limpiamos o borramos lo que haya. Para ello, ejecutamos los siguientes comandos para eliminar todas las reglas actuales:

```jsx
iptables -F
iptables -t nat -F
iptables -t mangle -F
```

Ahora establecemos las políticas por defecto para aceptar el tráfico.

```jsx
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
```

#### (3) Configurando la redirección de puertos

Para redirigir el tráfico de la IP pública, que en mi caso es: 192.168.11.42 en el puertos 80 hacia el servidor Nginx en la IP interna (10.10.10.30/24), usamos las siguientes reglas de iptables:

Redirige el tráfico entrante en el puerto 80 (HTTP) al servidor Nginx en la IP interna:

```bash

iptables -t nat -A PREROUTING -p tcp -d 192.168.11.42 --dport 80 -j DNAT --to-destination 10.10.10.30:80
```

#### (4) Configurar el enmascaramiento (Masquerading) para la salida a Internet

Para que las VM en la red interna (10.10.10.0/24) puedan acceder a Internet usando la IP pública de tu router (192.168.11.42), tenemos que agregar la siguiente regla en la cadena POSTROUTING:

```jsx
iptables -t nat -A POSTROUTING -s 10.10.10.0/24 -o *enp0s3* -j MASQUERADE
```

Ahora solo nos queda asegurarnos que las reglas establecidas en iptables permanecerán y no se borrarán. Para ello, instalamos:

```jsx
sudo apt install iptables-persistent  -y 
```

y después podemos hacer:

```jsx
sudo netfilter-persistent save
```

#### (5) Testeando el port forwarding

Ya lo tenemos todo listo. Solo nos falta testear. Para ello nos vamos al navegador del equipo anfitrión y tecleamos la IP pública de la VM router que, en mi caso, es: http:192.168.11.42:80

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

Como se puede ver, desde la IP pública del router he podido acceder a la página web del servidor de la red interna.
