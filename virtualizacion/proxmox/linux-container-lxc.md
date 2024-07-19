# Linux container - LXC

Tenemos dos tipos de contenedores: de sistemas y de aplicaciones.&#x20;

* **Contenedores de sistemas** - LXC: una forma de virtualización a nivel de sistema, muy similares a las VM donde podemos instalar servicios, acceder desde SSH, actualizar, etc. Al igual que las VM se pueden ejecutar múltiples sistemas aislados en un único equipo Linux. Una diferencia notable entre los LXC y las VM es que los contenedores no incluyen todo el S.O completo porque comparten el núcleo del S.O del host. Por esta razón son más ligeros y eficientes cuando se habla del uso de los recursos.
* **Contenedores de aplicaciones** - Docker: Se utilizan para el despliegue de aplicaciones dado que permite empaquetar las aplicaciones junto con sus dependencias, configuraciones y bibliotecas necesarias lo que permite ejecutarla aislada en cualquier entorno. Se centran en encapsular solo las aplicaciones y sus dependencias directas. Docker es uno de los ejemplos más conocidos de esta tecnología.

**Algunas características adicionales de los contenedores LXC son**:

1. LXC permite crear, gestionar y supervisar contenedores.&#x20;
2. Es posible crear un contenedor con una distribución Linux específica, así como iniciarlos y detenerlos contenedores y acceder a una consola para gestionarlos.
3. Pueden ser utilizados para aislar aplicaciones, hacer pruebas de software, pruebas entornos de desarrollo o despliegue de microservicios.
4. Brinda un aislamiento adecuado para muchas aplicaciones, aunque para entornos que requieren de mayor seguridad es recomendable  el uso de contenedores como la extensión de LXC llamada LXD o Docker.

#### Algunas características de los contenedores de aplicación son:

1. Garantizan que una aplicación se ejecute de la misma manera en cualquier entorno, ya sea en un entorno de desarrollo local, un servidor de prueba, o en un entorno de producción en la nube.&#x20;
2. Cada contenedor de aplicación se ejecuta en su propio espacio aislado, haciendo que las aplicaciones en diferentes contenedores no pueden interferir entre sí. Esto hace brinda mayor seguridad y estabilidad al facilitar el aislamiento de los fallos y prevenir conflictos de dependencias.
3. Comparten el kernel del S.O del host, haciéndolos más ligeros que las VM tradicionales, reduciendo el uso de recursos y permitiendo un mayor número de contenedores en un solo host.
4. Se pueden iniciar y detener con mucha facilidad, facilitando el despliegue y la escalabilidad de aplicaciones.&#x20;

Dentro de un LXC podemos instalar Docker.



### LXC en Proxmox

Falta explicar

#### Descargar una imagen de un contenedor

Falta explicar

#### Instalando un LXC&#x20;

Falta explicar

Un detalle a tener en consideración en la configuración del LXC es que debo asignar la IP de manera estática o no me funciona con el DHCP como debería.

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt="" width="563"><figcaption><p>Configuración de un LXC con Ubuntu 22.04 en Proxmox</p></figcaption></figure>

Una vez que tengamos instalado el contenedor, algo que es sumamente rápido, lo podremos ver en el nodo "pve". De hechom en esta imagen se pueden ver  dos contenedores: en lila el 102 (plesk) y el 104 (ubuntu2204) en verde que es el que acabamos de instalar.

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>Despliegue de VM y contenedores LXC en Proxmox</p></figcaption></figure>

Para ejecutar el contenedor puedes hacer click con el botón secundario del mouse y seleccionar `Start`.

En este tipo de contenedores podemos también:

* Configurar SSH, permitiendo en este caso el acceso a root (es el usuario que tenemos).
* Crear un nuevo "punto de montaje"&#x20;
* En general, hacer lo mismo que podemos hacer con una VM.

### Instalar Docker dentro del contenedor LXC

Tan fácil como hacer:

```
apt install docker.io
docker run -d -p 8080:80 --name nginx nginx
docker ps
```

En este caso le estamos diciendo que ejecute un contenedor de docker  con la imagen de Nginx y el puerto 8080 externo pero el 80 en el contenedor.

Nota: El problema que sigo teniendo es que solo he podido desplegar VM y contenedores en Proxmox teniendo Proxmox conectado en red "NAT" y haciendo un reenvío de puertos. Supuestamente las VM deberían tomar la IP por DHCP pero no lo hacen. Por tanto, le estoy asignando las IP de  modo estático. Los contenedores LXC si.&#x20;

Desconozco por qué no funciona teniendo Proxmox conectado a un adaptador puente. Proxmox si tiene salida a Internet pero las VM y los contenedores  no.
