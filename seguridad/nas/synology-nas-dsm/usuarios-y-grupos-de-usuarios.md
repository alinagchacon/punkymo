---
description: Como crear usuarios y grupos de usuarios dentro de Synology NAS
---

# Usuarios y grupos de usuarios

Los usuarios sirven para que los clientes se conectan al NAS (servidor) y puedan acceder a sus servicios. A través del panel de control de DSM se pueden gestionar estos usuarios.

También podemos gestionar un grupo grande de usuarios que queramos que tengan los mismos permisos de acceso.

### Primer paso - Acceder al Panel de control

Para crear y gestionar usuarios debes poseer el rol de administrador, el unico usuario que tenemos es el que tiene ese rol.

Primero de todo accedemos al panel de control, después nos dirigimos a **Usuario y grupo**.

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Usuario y grupo</p></figcaption></figure>

### Segundo paso - Creación del usuario

Para crear el usuario hacemos clic en el botón crear, desde ahí introducimos lo campos que nos pide.&#x20;

Justo debajo de los campos, podemos marcar que envie un correo de bienvenida el nuevo usuario, aunque si no tenemos el servidor de correo configurado no enviara nada.&#x20;

Tambíen podemos marcar la opción de que el usuario pueda cambiar la contraseña el mismo, es una opción recomendable si el usuario es de confianza, en este caso no la marcaremos, por lo tanto unicamente podran cambiarla los adminsitradores.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Tercer paso - Union a grupos

En la siguiente ventana encontraremos la opción de unir el usuario a un grupo. De forma predeterminada todos los usuarios pertenecen al grupo users. Este grupo no tiene ningún permiso espcial, unicamente quiere decir que es un usuario normal sin permisos de adminsitrador.
