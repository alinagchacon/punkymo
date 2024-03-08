---
description: Mensajería instantánea
---

# Ejabberd

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

Modificamos el host para que tome el dominio del servidor. Para ello:

```
nano /opt/ejabberd/conf/ejabberd.yml
```

<figure><img src="../.gitbook/assets/image (285).png" alt=""><figcaption><p>Cambiar el host al dominio de tu servidor</p></figcaption></figure>

### Cuentas&#x20;

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

