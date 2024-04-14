---
description: en Docker
---

# Instalando Wordpress

Pudiéramos instalar Wordpress utilizando una de las plantillas que nos proporciona Portainer. Para ello nos vamos a `Application templates`, buscamos Wordpress y hacemos clic sobre él para comenzar.

Lo primero que hace es pedirnos el nombre de la aplicación, contraseña de root, etc. Una vez hecho, “desplegamos la pila”.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Application templates en Portainer</p></figcaption></figure>



Una vez finalizado el proceso inicial de instalación, podemos acceder a la sección de Stacks y veremos nuestro docker-compose con la aplicación de WordPress.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>WordPress en el listado de stacks de Portainer</p></figcaption></figure>

A clicar encima de miwp, podrás desplegar en detalle que está compuesto por dos contenedores: uno para la base de datos y el otro para el wordpress como tal.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Una vez lo tengamos, podemos acceder al sitio web de WordPress a través de la misma IP de nuestra VM pero utilizando el puerto que nos muestra en Portainer:

```
http://192.168.40.99:32768/wp-admin/
```

En este punto, es que comienza la instalación de WordPress

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Listos para instalar WordPress</p></figcaption></figure>

Una vez terminado el proceso, que es muy rápido, ya tendremos el acceso a nuestro sitio web:

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>En nuestro dashboard de WordPress</p></figcaption></figure>

To be continued ...
