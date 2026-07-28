---
description: Instalación de MySQL
---

# Servidores MySQL

MySQL es el sistema de gestión de bases de datos relacional de código abierto y el más extendido que se tiene actualmente. Fue inicialmente desarrollado por MySQL AB, más tarde adquirida por Sun MicroSystems y finalmente comprada por Oracle Corporation, que ya tenía un motor propio InnoDB para MySQL.<br>

Este sistema de gestión de bases de datos cuenta con una doble licencia:

* Por una parte es de código abierto,
* Por otra parte, cuenta con una versión comercial gestionada por la compañía Oracle

Entre las características más relevantes de MySQL tenemos:

1. **Código abierto**. muy accesible por lo que una gran cantidad de programadores de desarrollo web lo utilizan.
2. **BD Relacional.** los datos están organizados en tablas relacionadas, lo que facilita la organización y la integridad de los mismos.
3. **Arquitectura Cliente-Servidor**: basa su funcionamiento en el modelo cliente - servidor.
4. **Compatibilidad con SQL**: MySQL se basa en SQL, que es un lenguaje de consulta estructurado - Structured Query Language, por lo que es compatible con el lenguaje SQL.
5. **Vistas y procedimientos**: permite configurar vistas personalizadas y almacenar procedimientos que incrementan la eficacia de las implementaciones.
6. **Compatibilidad.** funciona en Windows, Linux y macOS.
7. **Triggers.** permite automatizar tareas dentro de BD (al producirse un evento otro es lanzado).
8. **Transacciones**. Permite trabajar con transacciones, fundamental para todas aquellas aplicaciones que requieren seguridad en la gestión de los datos. Cuando hablamos de transacciones estamos hablando de la actuación de varias operaciones en la BD. El sistema avala que todos los procedimientos se puedan establecer de manera correcta o por el contrario no realiza ninguna de ellas.
9. **Rendimiento y escalabilidad.** alta capacidad para la manipulación de grandes volúmenes de datos, bueno para aplicaciones de alto rendimiento.
10. **Comunidad activa.** cuenta con una gran comunidad que desarrolla y mejora continuamente el sistema, gracias a ser de código abierto.

MySQL es muy utilizado en entornos de desarrollo como es el caso de la llamada pila LAMP: Linux, Apache, MySQL, PHP/Python/Perl. Se utiliza en combinación con otros lenguajes como PHP siendo muy popular en los entornos web.

## Características del entorno de instalación

Para instalar MySQL voy a utilizar una VM con Ubuntu Server 20.04.6 (Focal). De hecho, la VM que tengo como cliente dentro de la VM de Proxmox.

<figure><img src="../.gitbook/assets/image (857).png" alt="" width="563"><figcaption><p>La instalación de MySQL se hará sobre la VM Cliente con IP 10.10.10.2/24</p></figcaption></figure>

{% hint style="info" %}
Puedes obtener más información de las características del entorno de trabajo accediendo al apartado de Proxmox > [red Interna](../virtualizacion/proxmox/red-interna.md).
{% endhint %}

## Instalación

Actualizamos el sistema:

```
sudo apt update
supo apt upgrade
```

instalamos el servicio de MySQL:

```
sudo apt install mysql-server
```

Ejecutamos ahora la secuencia de comandos de seguridad:

```
sudo mysql_secure_installation
```

Ejecutando este comando seremos guiados a través del proceso que brindará seguridad en la instalación de MySQL.

1. Nos pregunta si queremos configurar el complemento de validación de la contraseña, que puede usar para probar la seguridad de la contraseña de MySQL.
   1.  Si decidimos que si, entonces nos solicitará elegir un nivel de validación de contraseña.

       1. El nivel más alto de validación de la contraseña se consigue seleccionando la opción `2`, que se corresponde con una contraseña de al menos 8 caracteres: incluyendo una combinación de mayúsculas, minúsculas, números y caracteres especiales

       <figure><img src="../.gitbook/assets/image (91).png" alt="" width="563"><figcaption><p>Iniciando el proceso de configuración con seguridad</p></figcaption></figure>

El siguiente paso nos pide que seleccionemos el nivel de seguridad en la contraseña:

<figure><img src="../.gitbook/assets/image (92).png" alt="" width="563"><figcaption><p>Podemos seleccionar diferentes niveles de seguridad en la contraseña</p></figcaption></figure>

Por defecto, la instalación de MySQL proporciona un usuario "anónimo" que no debemos permitir en entornos de producción (como puede ser el proyecto de síntesis):

<figure><img src="../.gitbook/assets/image (93).png" alt="" width="563"><figcaption><p>Eliminar el usuario anónimo</p></figcaption></figure>

El siguiente paso nos pregunta por el tipo de acceso que le daremos al usuario root, dado que éste solo debe poder conectarse desde "localhost" para evitar que nadie pueda "pillar" la contraseña de root por la red:

<figure><img src="../.gitbook/assets/image (94).png" alt="" width="563"><figcaption><p>Impedir que el usuario root pueda conectarse remotamente</p></figcaption></figure>

Podemos eliminar la DB test que también debe eliminarse si estamos configurando un entorno de producción:

<figure><img src="../.gitbook/assets/image (95).png" alt="" width="563"><figcaption><p>Eliminando o no la DB test</p></figcaption></figure>

Y el último paso

<figure><img src="../.gitbook/assets/image (96).png" alt="" width="563"><figcaption><p>Recargando los privilegios</p></figcaption></figure>

Tanto si hemos realizado estos pasos como si no, lo cierto es que todavía no le hemos otorgado una contraseña a nuestro usuario "root".

### Establecer la contraseña para el usuario "root"

Para establecer una contraseña para el usuario root debemos acceder primero a MySQL:

```
sudo mysql
```

Podemos usar `ALTER USER` para configurar la contraseña del usuario 'root':

Con `ALTER USER` podemos establecer la contraseña para el **root** de MySQL y que pueda autenticarse con el complemento `caching_sha2_password`.

Este complemento es el preferido de MySQL ([Documentación oficial de MySQL](https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password)) para la autenticación dado que proporciona un cifrado más seguro a la contraseña. Sin embargo, aplicaciones de PHP, como es el caso de phpMyAdmin, no funcionan de forma fiable con `caching_sha2_password`.

Por tanto, como la idea es usar MySQL con PHP, tendremos que establecer la contraseña del usuario **root** con `mysql_native_password` ([Documentación oficial de MySQL](https://dev.mysql.com/doc/refman/8.4/en/native-pluggable-authentication.html)).

```
 ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

### Actualizar los privilegios

Una vez que hemos creado o modificado la contraseña, nos tenemos que asegurar de actualizar los privilegios para que los cambios surtan efecto:

```sql
mysql> FLUSH PRIVILEGES;
```

### Salir de MySQL

Para salir de MySQL usamos el siguiente comando:

```bash
mysql> exit;
```

Podemos volver a acceder a MySQL haciendo:

```
mysql -u root -p
mysql> show databases;
mysql> SELECT user,authentication_string,plugin,host FROM mysql.user;
```

Con este último comando nos debe mostrar algo como lo siguiente, donde podemos ver que los usuarios que trae por defecto tienen establecida su contraseña con el complemento: caching\_sha2\_password y el root con mysql\_native\_password:

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption><p>Comprobando los métodos de autenticación empleados por cada usuario</p></figcaption></figure>

## Crear un usuario nuevo

Ahora vamos a crear un usuario 'kirby' con una contraseña segura:

```
mysql> CREATE USER 'kirby'@'localhost' IDENTIFIED BY 'my_password';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'kirby'@'localhost' WITH GRANT OPTION;
mysql> EXIT;
```

### Comprobaciones

Comprobemos que todo esté correcto. Para ello podemos hacer:

```
systemctl status mysql.service
```

Otro tipo de comprobación adicional sería establecer conexión con la base de datos usando la herramienta `mysqladmin`, que es un cliente que le permite ejecutar comandos administrativos.

```
sudo mysqladmin -p -u root version
```

Y nos mostrará el siguiente pantallazo:

<figure><img src="../.gitbook/assets/image (98).png" alt="" width="563"><figcaption><p>Pantallazo de información con la herramienta mysqladmin</p></figcaption></figure>

## Importar una DB

Dado que estoy trabajando en el equipo Cliente que es un servidor Ubuntu 20.04 y teniendo el servicio SSH habilitado he utilizado el comando SCP para enviar el fichero desde mi equipo anfitrión. Por supuesto, he tenido que crear un reenvío de puertos porque tengo Proxmox en una VM conectada a la red: NAT.

<figure><img src="../.gitbook/assets/image (90).png" alt=""><figcaption><p>Reenví ode puerto en Proxmox</p></figcaption></figure>

Para enviar el archivo de la DB:

```
scp -C -P 8080 users.sql kirby@192.168.1.13:/home/kirby/ 
```

Ahora tenemos que crear una DB con el mismo nombre de la DB que queremos importar. Para ello:

```
mysql> create database users;
```

Y ya podremos importar el archivo .sql en la DB

```
mysql -u usuario -p nombre_basededatos < data.sql
```
