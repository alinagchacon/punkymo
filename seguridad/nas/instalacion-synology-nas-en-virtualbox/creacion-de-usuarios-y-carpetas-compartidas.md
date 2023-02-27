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

<figure><img src="../../../.gitbook/assets/image (3) (1) (2).png" alt=""><figcaption><p>Panel de gestión de carpetas compartidas</p></figcaption></figure>

### **Segundo paso - Creación de la carpeta**

Una vez estamos en las carpetas compartidas hacemos clic en el botón de **Crear**. En el desplegable elegiremos la opción de **Crear una carpeta compartida.**

Se nos abrirá una ventana donde debemos introducir diferentes datos que nos piden, estos son el nombre de la carpeta, una descripción opcional y el volumen donde queremos que se guarde, en este caso únicamente tenemos por lo que no tenemos opción de elegir.

Después tenemos varias casillas que podemos marcar o no, esto afectara al comportamiento y las funciones de la carpeta.

La primera de ellas nos permite ocultar la carpeta en los **"Mis sitios de red"**. Esto quiere decir que cuando accedamos por el protocolo SMB aun no teniendo permisos para acceder a la carpeta podremos ver el nombre de esta. Dependiendo de las circunstancias la dejaremos activada o no.

La segunda opción indica que si queremos o**cultar las subcarpetas y archivos a los usuarios sin permiso**, esto quiere decir que si tenemos usuarios con distintos permisos en el interior de la carpeta se oculten o no a este.&#x20;

Por ultimo nos encontramos la opción de la **Papelera de reciclaje** esto quiere decir que lo que eliminemos da la carpeta con esta opción habilitada se almacenara en la papelera en vez de elimanarse permanentemente. Esta opción la dejaremos habilitada. La opción de justo debajo quiere decir que si queremos restringir la papelera a solo administradores, deshabilitaremos esta opción para que todos los usuarios con permisos a esa carpeta puedan acceder.

<figure><img src="../../../.gitbook/assets/image (17) (2).png" alt=""><figcaption><p>Información básica de carpetas</p></figcaption></figure>

### **Tercer paso - Cifrado de carpeta**

En este paso nos da la opción de cifrar la carpeta con una clave, por defecto esta deshabilitada porque reduce el rendimiento de la carpeta. Además para acceder a la carpeta ya usamos el usuario y contraseña.&#x20;

<figure><img src="../../../.gitbook/assets/image (4) (4).png" alt=""><figcaption><p>Cifrado de carpetas</p></figcaption></figure>

### Cuarto paso - Configuración avanzada

Las opciones de esta paso únicamente están disponibles para los volúmenes con el sistema de archivos **btrfs**. Esta opción permite comprobar la integridad de los datos y poder habilitar una cuota de espacio a la carpeta compartida de forma predeterminada. Mas adelante podremos habilitar las cuotas por cada usuario que tenga acceso a la carpeta.

<figure><img src="../../../.gitbook/assets/image (7) (3).png" alt=""><figcaption><p>Configuración avanzada de la carpeta compartida</p></figcaption></figure>

### Quinto paso - Confirmar la configuración

En este paso confirmaremos que los datos que hemos introducido son correctos. En el caso contrario podremos volver atrás para modificarlos.

<figure><img src="../../../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption><p>Confirmar configuración</p></figcaption></figure>

### Sexto paso - Permisos de usuario

Una vez revisada la configuración, en este paso nos encontramos con la opción de asignar permisos de **lectura/escritura** a los usuarios de nuestro NAS. De momento únicamente tenemos el usuario de administrador, este siempre tiene acceso de lectura y escritura, aunque también se los podemos denegar. Si denegamos el acceso al administrado únicamente podremos gestionar los usuarios que acceden a ella y no ver su contenido.

<figure><img src="../../../.gitbook/assets/image (1) (4) (1).png" alt=""><figcaption><p>Configurar permisos de usuario</p></figcaption></figure>

### Séptimo paso - Finalizar creación de la carpeta

Una vez apliquemos los cambios la carpeta se creara y nos aparece en el panel de las carpetas compartidas. Desde ahí podremos ver características de la carpeta, su espacio, si tiene papelera, etc.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption><p>Visualizar carpeta desde el panel de control</p></figcaption></figure>

### Octavo paso - Visualizar interior de la carpeta

Para ver el interior de la carpeta podemos acceder a la aplicación llamada **File Station**. Esta la podremos encontrar en el escritorio o donde se encuentran todas las aplicaciones instaladas. Desde ahí podremos ver todas las carpetas raíz que tengamos compartidas. Esto se aplica a todos los usuarios.&#x20;

Si accedemos a su interior podemos ver que tenemos la **Papelera de reciclaje**, sin ninguna otra carpeta. Para crear una carpeta dentro debemos hacer clic en **Crear** y a **Crear Carpeta.** A esta carpeta le asignare el nombre de **Prueba2**.

Esta aplicación la usaremos siempre que queramos acceder a las carpetas desde la interfaz web, también podemos descargar y cargar todo tipo de archivos como si se tratara de una nube.

<figure><img src="../../../.gitbook/assets/image (13) (2).png" alt=""><figcaption><p>Aplicación File Station</p></figcaption></figure>

## Creación de usuarios

### Primer paso - Acceder al Panel de control

Para crear y gestionar usuarios debes poseer el rol de administrador, el único usuario que tenemos es el que tiene ese rol.

Primero de todo accedemos al panel de control, después nos dirigimos a **Usuario y grupo**. **** Desde ese panel podremos ver todos los usuarios creados, durante la instalación del NAS se crean dos usuarios extras, el **admin** y el **guest**. Por defecto aparecen deshabilitados. Es recomendable nunca activar el usuario de **admin** ya que nuestro NAS puede sufrir ataques de fuerza bruta.

<figure><img src="../../../.gitbook/assets/image (11) (2) (1) (2).png" alt=""><figcaption><p>Usuario y grupo</p></figcaption></figure>

### Segundo paso - Creación del usuario

Para crear el usuario hacemos clic en el botón crear, desde ahí introducimos lo campos que nos pide.&#x20;

Justo debajo de los campos, podemos marcar que envie un correo de bienvenida el nuevo usuario, aunque si no tenemos el servidor de correo configurado no enviara nada.&#x20;

Tambíen podemos marcar la opción de que el usuario pueda cambiar la contraseña el mismo, es una opción recomendable si el usuario es de confianza, en este caso no la marcaremos, por lo tanto unicamente podran cambiarla los adminsitradores.

<figure><img src="../../../.gitbook/assets/image (1) (2) (1).png" alt=""><figcaption><p>Información del usuario</p></figcaption></figure>

### Tercer paso - Unión a grupos

En la siguiente ventana encontraremos la opción de unir el usuario a un grupo. De forma predeterminada todos los usuarios pertenecen al grupo **users**. Este grupo no tiene ningún permiso especial, únicamente quiere decir que es un usuario normal sin permisos de administrador. En el caso de tener otros grupos, podremos unirlo en el momento de le creación del usuario. Se puden unir posteriormente si es necesario.

<figure><img src="../../../.gitbook/assets/image (4) (1) (3).png" alt=""><figcaption><p>Grupos de usuarios</p></figcaption></figure>

### Cuarto paso - Asignación de carpetas compartidas

En este paso nos encontraremos las carpetas compartidas que tengamos en nuestro NAS, a las que podremos **asignar los permisos** de **lectura/escritura o únicamente lectura.** Desde aquí podemos ver que tenemos únicamente una carpeta. Por defecto nos aparece sin acceso ya que la politica del grupo **users** no contempla cuales son los permisos por defecto. A este usuario le asignaremos los permisos de **lectura/escritura**.

<figure><img src="../../../.gitbook/assets/image (2) (1) (2).png" alt=""><figcaption><p>Permisos de usuarios a las carpetas comaprtidas</p></figcaption></figure>

### Quinto paso - Cuotas de usuario

En este paso podremos asignar una cuota de espacio al usuario. Esto quiere decir que si el usuario se pasa de la cantidad de espacio asignado no le dejará copiar mas archivos. En este caso no asiganremos ninguna cuota.

<figure><img src="../../../.gitbook/assets/image (3) (2) (2).png" alt=""><figcaption><p>Cuotas de usuario</p></figcaption></figure>

### Sexto paso - Asignar permisos de aplicaciones

En este paso podremos elegir las aplicaicones a la que queremos que tenga acceso este usuario. En el caso de que no nos convenga que acceda a alguna de ellas unicamente debemos marcar la casilla de **Denegar**. En este caso este usuario tendrá acceso a todas las aplicaciones que se muestran por pantalla.

<figure><img src="../../../.gitbook/assets/image (8) (3).png" alt=""><figcaption><p>Permisos de aplicaciones</p></figcaption></figure>

### Séptimo paso - Limitar velocidad transferencia

En este paso podremos limitar de las aplicaciones de acceso a las carpetas compartidas. Limitar la velocidad de transferencia nos puede ayudar a saturar la red si tenemos muchos usuarios. En este caso no especificaremos ningun limite ya que no necesitamos limitar la velocidad de este usuario.

<figure><img src="../../../.gitbook/assets/image (5) (2) (1).png" alt=""><figcaption><p>Establecer limite de velocidad de transferencia</p></figcaption></figure>

### Octavo paso - Confirmar los datos

Por ultimo comprobaremos que los datos de la creación del usuario esten correctos. En el caso contrario los podremos modificar volviendo para atrás.

<figure><img src="../../../.gitbook/assets/image (14) (1) (2).png" alt=""><figcaption><p>Confirmar configuración del usuario</p></figcaption></figure>

Una vez creado el usuario este puede ser modificado, esto puede hacerse desde el panel de control, en el mismo apartado donde se crean los usuarios.
