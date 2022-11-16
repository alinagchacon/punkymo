---
description: JLM
---

# Creación de usuarios y carpetas compartidas

Los usuarios sirven para que los clientes se conectan al NAS (servidor) y puedan acceder a sus servicios. A través del panel de control de DSM se pueden gestionar estos usuarios.

También podemos gestionar un grupo grande de usuarios que queramos que tengan los mismos permisos de acceso.

Las carpetas compartidas es desde donde gestionaremos los **** directorios básicos en los que puede almacenar y administrar archivos y carpetas del NAS.

No debemos confundir con las carpetas que pueda crear el usuario en su interior. Estas carpetas comaprtidas nos referimos a la raíz, a la que podemos asignarle los usuarios que pueden acceder y sus permisos. También se pueden asignar permisos a las subcarpetas.

## Creación de carpetas compartidas

### Primer paso - Acceder al al Panel de control

Para la creación de carpetas compartidas debes ser un usuario con permisos de administrador, para ello usaremos el único usuario que tenemos.&#x20;

Lo primero que debemos hacer para crear una carpeta es acceder al panel de control, después haremos clic sobre **Carpetas compartidas.**  Desde ahi podemos observer las carpetas compartidas que tenemos, en este momento no tenemos ninguna, en el momento de crearlas apareceran ahi y podremos gestionarlas de una forma mas ordenada.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Panel de gestión de carpetas compartidas</p></figcaption></figure>

### **Segundo paso - Creación de la carpeta**

Una vez estamos en las carpetas compartidas hacemos clic en el botón de **Crear**. En el desplegable elegiremos la opción de **Crear una carpeta compartida.**

Se nos abrirá una ventana donde debemos introducir diferentes datos que nos piden, estos son el nombre de la carpeta, una descripción opcional y el volumen donde queremos que se guarde, en este caso únicamente tenemos por lo que no tenemos opción de elegir.

Después tenemos varias casillas que podemos marcar o no, esto afectara al comaprtamiento y las funciones de la carpeta.

La primera de ellas nos permite ocultar la carpeta en los **"Mis sitios de red"**. Esto quiere decir que cuando accedamos por el protocolo SMB aun no teniendo permisos para acceder a la carpeta podremos ver el nombre de esta. Dependiendo de las circunstancias la dejaremos activada o no.

La segunda opción indica que si queremos ocultar las subcarpetas y archivos a los usuarios sin permiso, esto quiere decir que si tenemos usuarios con distintos permisos en el interior de la carpeta se oculten o no a este.&#x20;

****

## Creación de usuarios

### Primer paso - Acceder al Panel de control

Para crear y gestionar usuarios debes poseer el rol de administrador, el único usuario que tenemos es el que tiene ese rol.

Primero de todo accedemos al panel de control, después nos dirigimos a **Usuario y grupo**. **** Desde ese panel podremos ver todos los usuarios creados, durante la instalación del NAS se crean dos usuarios extras, el **admin** y el **guest**. Por defecto aparecen deshabilitados. Es recomendable nunca activar el usuario de **admin** ya que nuestro NAS puede sufrir ataques de fuerza bruta.

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Usuario y grupo</p></figcaption></figure>

### Segundo paso - Creación del usuario

Para crear el usuario hacemos clic en el botón crear, desde ahí introducimos lo campos que nos pide.&#x20;

Justo debajo de los campos, podemos marcar que envie un correo de bienvenida el nuevo usuario, aunque si no tenemos el servidor de correo configurado no enviara nada.&#x20;

Tambíen podemos marcar la opción de que el usuario pueda cambiar la contraseña el mismo, es una opción recomendable si el usuario es de confianza, en este caso no la marcaremos, por lo tanto unicamente podran cambiarla los adminsitradores.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>Información del usuario</p></figcaption></figure>

### Tercer paso - Unión a grupos

En la siguiente ventana encontraremos la opción de unir el usuario a un grupo. De forma predeterminada todos los usuarios pertenecen al grupo **users**. Este grupo no tiene ningún permiso especial, únicamente quiere decir que es un usuario normal sin permisos de administrador. En el caso de tener otros grupos, podremos unirlo en el momento de le creación del usuario. Se puden unir posteriormente si es necesario.

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Grupos de usuarios</p></figcaption></figure>

### Cuarto paso - Asignación de carpetas compartidas

En este paso nos ecnontraremos las carpetas compartidas que tengamos en nuestro NAS, a las que desde este paso podemos asignar los permisos de lectura/escritura. En este caso de momento no tenemos ninguna carpeta, por lo tanto no veremos nada. Posteriormente crearemos una carpeta y se la comaprtiremos al usuario.

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Asignación de carpetas</p></figcaption></figure>



<mark style="color:red;">To be continued...</mark>

