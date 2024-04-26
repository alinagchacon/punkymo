---
description: Mensajería instantánea
---

# Ejabberd

Se trata de un servidor de mensajería instantánea de código abierto. Válido para plataformas Unix BSD, GNU/Linux, Microsoft Windows entre otras. Para la comunicación instantánea se utiliza XMPP.

## Instalación y configuración

En mi caso estoy utilizando dos VM conectadas en red NAT. El equipo donde estoy configurando ejabberd es un servidor de Ubuntu 22.04 donde también tengo funcionando el servicio de DNS con el dominio `haven.haven.local`.

## Comandos básicos

Algunos comandos básicos para usar ejabberd:

Ver el estado actual del servicio ejabberd:

```
sudo /opt/ejabberd-24.02/bin/ejabberdctl status
```

Listado de usuarios creados en un nodo o server (roster)

```
sudo /opt/ejabberd-24.02/bin/ejabberdctl registered-users haven.haven.local
```

Parar el servicio ejabberd

```
sudo /opt/ejabberd-24.02/bin/ejabberdctl stop
```

Iniciar el servicio ejabberd

```
sudo /opt/ejabberd-24.02/bin/ejabberdctl start
```

## Configuración básica

Vamos a editar el archivo `ejabber.yml`  y modificaremos algunos parámetros.&#x20;

```
nano /opt/ejabberd/conf/ejabberd.yml
```

Añadimos el dominio del servidor al `host,` en mi caso `haven.haven.local`.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>Cambiar el host al dominio del servidor</p></figcaption></figure>

Buscamos el apartado `trusted_network` y lo ponemos en `all`, esto es:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Permitimos todo como "trusted_network"</p></figcaption></figure>

Ahora le vamos a dar permisos de administrador al usuario `admin` que hemos creado previamente:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Permisos de administración al usuario admin</p></figcaption></figure>

El **acceso web** viene _habilitado por defecto_ por las siguientes lineas:

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Puerto 5280 del acceso web de ejabberd</p></figcaption></figure>

### Creación de cuentas de usuario&#x20;

La primera cuenta a crear es la cuenta de `admin` y  la utilizaremos para administrar nuestro ejabberd xmpp vía web. Para crear una cuenta necesitamos el comando `ejabberdctl.`

En mi caso, los parámetros que he utilizado son:



|            |                        |
| ---------- | ---------------------- |
| register   | crear cuenta           |
| usuario    | admin                  |
| dominio    | haven.haven.local      |
| contraseña | la contraseña de admin |

La siguiente línea de comando creamos un usuario administrador del sistema:

```
sudo ejabberdctl register admin haven.haven.local tupassword
sudo ejabberdctl register punky haven.haven.local tupassword
```

<figure><img src="../.gitbook/assets/image (286).png" alt=""><figcaption><p>Creando un usuario admin en ejabberd</p></figcaption></figure>

Si quieres visualizar los usuarios registrados en el sistema puedes hacer:

```
sudo ejabberdctl registered-users haven.haven.local
```

Y te mostrará la lista de usuarios creados.

## Acceso vía web

En mi caso, me voy a conectar desde la VM que está haciendo de cliente y utilizo la IP del servidor con el puerto por defecto de ejabberd: `5280`.&#x20;

```
https://192.168.6.100/admin
```

Nos saldrá una ventana donde nos pide el `usuario` y `contraseña` para acceder. Para ello usaremos el usuario `admin` y ya tendremos acceso a la web de administración.

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Web de administración de ejabberd</p></figcaption></figure>

Si clicamos en `Dominios Virtuales` vemos el dominio que hemos creado:

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1).png" alt=""><figcaption><p>Dominio haven.haven.local </p></figcaption></figure>

Si clicamos en el dominio `haven.haven.local` accedemos a otras opciones de configuración, como muestra la siguiente imagen.

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption><p>Dashboard de configuración de ejabberd</p></figcaption></figure>

Podemos ver y crear usuarios para el sistema. Modificar las contraseñas, eliminar los mensajes, crear y acceder  a las salas de chat, ver las estadísticas, etc.

## Pidgin

Para comprobar la comunicación entre los usuarios, utilizaremos la herramienta `pidgin`. Para ello vamos a  instalar pidgin tanto en la VM cliente como  en la VM servidor que tiene instalado y configurado el `ejabberd`. Así que instalamos pidgin con el comando:

```
sudo apt install pidgin
```

Editamos el archivo /etc/hosts de la VM cliente y escribimos la IP del servidor apuntando al dominio en cuestión:

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Archivo /etc/hosts en el equipo cliente</p></figcaption></figure>

Ahora accedemos a Pidgin y añadimos uno de los usuarios que hemos creado en ejabberd.

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption><p>Pidgin</p></figcaption></figure>

<mark style="color:orange;">To be continued ...</mark>

## Links

* [https://www.process-one.net/blog/ejabberd-xmpp-server-useful-configuration-steps/](https://www.process-one.net/blog/ejabberd-xmpp-server-useful-configuration-steps/)&#x20;
