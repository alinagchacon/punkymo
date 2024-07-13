---
description: Rsync
---

# Rsync en Truenas

Rsync es una herramienta por línea de comandos, de Linux que permite la sincronización y la transferencia de archivos.  El nombre de la herramienta proviene de `remote sync` dada el uso para el que fue diseñado: sincronizar y copiar archivos entre sistemas remotos o locales. Es una herramienta rápida y segura para copiar datos a otro sistema, para copia de seguridad o migración de datos.&#x20;

_**Nota**: si quieres más información sobre el uso de rsync en Linux ve a:_ [_https://punkymo.gitbook.io/miwiki/seguridad/backups/rsync_](https://punkymo.gitbook.io/miwiki/seguridad/backups/rsync)&#x20;

Para configurar una tarea rsync necesitamos configurar un sistema host y otro remoto, asumiendo un sistema TrueNAS para las configuraciones de Host y Remota.

Dado que Rsync proporciona la capacidad de enviar o extraer datos:

* cuando se utiliza rsync para enviar, los datos se copian desde un sistema host a un sistema remoto.&#x20;
* cuando se utiliza rsync para extraer, los datos se extraen de un sistema remoto. Luego se coloca en el sistema anfitrión.

Por otra parte, Truenas posee otros requisitos  que dependen  de si eligimos el modo Módulo o SSH rsync. A la hora de configurar un servicio Rsync debemos tener en cuenta que:

* Antes de crear una tarea rsync en el sistema host, debe crear un módulo en el sistema remoto.&#x20;
* El sistema remoto debe tener activado el servicio rsync.&#x20;
* Cuando TrueNAS es el sistema remoto, creamos un módulo.

## Configurar Rsync para sincronizar dos Truenas

Vamos a realizar la sincronización de dos sistemas Truenas. He clonado la VM con Truenas y ahora dispongo de dos VM casi idénticas (he modificado algunos archivos y usuarios). Ambas VM están en modo Adaptador Puente por lo tanto tengo comunicación entre mi propio equipo anfitrión o host y las dos VM con Truenas.

| Equipo              | IP            |
| ------------------- | ------------- |
| Anfitrión           | 192.168.1.15  |
| Truenas 1           | 192.168.1.145 |
| Truenas 2 (clonado) | 192.168.1.113 |

### Configurando el Truenas 1

**(1) Crear un nuevo dataset**&#x20;

Primeramente vamos a considerar que este Truenas es el host, así que lo primero que hice fue crear un dataset dentro de KirbyPool, esto es: &#x20;

`Storage > Pool > KirbyPool` y creamos el dataset que he llamado: `BACKUP`

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Crear un dataset BACKUP para almacenar las copias de los usuarios</p></figcaption></figure>



**(2) Servicio Rsync activado**

Como segundo paso nos vamos a services y verificamos que tengamos activamos el servicio de Rsync, y lo editamos.  Estaríamos en:  `Services > Rsync > Rsync Module > Add`.

Nos encontramos con una pestaña con el puerto por defecto: <mark style="color:red;">873</mark> que pudiéramos modificar aunque no lo haremos.&#x20;

Nos vamos a la segunda pestaña que nos encontramos: `Rsync Module` donde vamos a crear un nuevo módulo.  En esta ventana que nos aparece tenemos que seleccionar:

* Seleccionamos el conjunto de datos de origen para usar con la tarea rsync. Esto es, el path hacia el directorio que queremos sincronizar. En mi caso sería: `/mnt/KirbyPool/BACKUP`.
* Habilitar este módulo para utilizarlo con Rsync.&#x20;
* Le damos permisos de lectura y escritura al módulo que estamos activando
* &#x20;Seleccionamos una cuenta de usuario para ejecutar la tarea rsync. Será el usuario root quien tenga el permiso para realizar la sincronización.

Nos queda de la siguiente manera:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Configurando el módulo Rsync en: Services > Rsync > Rsync Module > Add</p></figcaption></figure>

**(3) Crear una tarea Rsync**

El tercer paso sería agregar una tarea Rsync. Para ello nos vamos a: `Tasks > Rsync Tasks`, y clicamos en `Add`.&#x20;

En la ventana de opciones que se nos muestra tenemos que definir:&#x20;

* El conjunto de datos de origen para la tarea rsync
* Seleccionar la cuenta de usuario.
* Seleccionar la dirección para la tarea rsync, Push o Pull.
* Definir la programación de la tarea. En qué momento y con qué frecuencia se va a ejecutar la sincronización.
* Ingresamos la dirección IP del host remoto o el nombre.&#x20;
* Configuramos las opciones restantes según necesitemos.

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption><p>Rsync task en el Truenas 1 con IP 192.168.1.145</p></figcaption></figure>

Podemos ejecutar la tarea rsync yendo a: Tareas > Tareas Rsync y haciendo clic en Ejecutar ahora.\


<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Ejecutar la tarea de sincronización con Rsync</p></figcaption></figure>

Pero antes de ejecutar la tarea nos vamos al otro sistema: Truenas 2

### Configurando el Truenas 2 (clonado)

En este otro Truenas realizamos los mismos pasos que en el caso anterior. Lo primero será ir a la sección de `Storage`. Esto es,

**(1) Creamos el pool llamado Backup**

En `Storage > Pool > KirbyPool` creamos el dataset que he llamado también: `BACKUP`

**(2) Servicio Rsync activado**

En `Services > Rsync > Rsync Module > Add` y  verificamos que tengamos activamos el servicio de Rsync y lo editamos.&#x20;

**(3) Crear una tarea Rsync**

En `Tasks > Rsync Tasks`, y clicamos en `Add` para crear una nueva tarea.&#x20;

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1).png" alt=""><figcaption><p>Rsync task en el Truenas 2 con IP 192.168.1.13</p></figcaption></figure>

Una vez tengamos configurada la tarea Rsync en el Truenas 2 con IP 192.168.1.113 podremos ejecutarla y vemos que, efectivamente, se sincroniza con el Truenas 1 que tiene dirección IP 192.168.1.145.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption><p>Tarea rsync que se ejecuta correctamente</p></figcaption></figure>

## Configurar un Truenas y un servidor Linux en VM

¿Qué sucede si no tenemos dos servidores Truenas para realizar las copias de seguridad? Supongamos que tenemos un servidor web en Linux con nuestra aplicación web y queremos hacer un backup de todo en el  Truenas. Veamos que podemos hacer.

Voy a utilizar el Truenas 1 con IP 192.168.1.145 que ya está configurado. Desde el servidor web de Linux, que tiene una IP 192.168.1.  ejecuté el comando de rsync:

```
rsync -av -zz -e  "/bin/ssh" --progress abackup pepe@192.168.40.145:~/BACKUP/
```

Con esta línea solo no basta porque:

* nos pide que escribamos manualmente la contraseña del usuario que estamos utilizando para conectarnos al Truenas
* la copia se está realizando de modo manual.

Por tanto tendremos que automatizar la copia de seguridad. Para ello debemos agregar una tarea `rsync` a `cron` en el servidor Linux para que se ejecute periódicamente.&#x20;

Nos vamos al crontab con:

```bash
crontab -e
```

Agregamos una línea como la siguiente para ejecutar la copia de seguridad todos los días a las 4h:

```perl
copia 4 * * * rsync -avz --delete abackup pepe@192.168.1.145:~/BACKUP/
```

Guardamos y cerramos el  `crontab` para activar la tarea.

Así es que tendríamos una copia de seguridad automatizada desde el servidor Linux hacia el TrueNAS utilizando `rsync` y SSH.&#x20;

_Nota: En la sección_ [_https://punkymo.gitbook.io/miwiki/seguridad/backups/rsync_](https://punkymo.gitbook.io/miwiki/seguridad/backups/rsync) _tienes varios ejemplos de como podemos realizar este tipo de copias de seguridad utilizando un script y el crontab._

