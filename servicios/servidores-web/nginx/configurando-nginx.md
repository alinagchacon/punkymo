# Configurando NGINX

Nginx está compuesto por una serie de módulos que son controlados por directivas especificadas en el archivo de configuración.

Estas directivas se dividen en simples y de bloque.

* Una <mark style="color:blue;">`directiva simple`</mark> consiste en el nombre y los parámetros separados por espacio y terminados en punto y coma <mark style="color:blue;">`;`</mark>
* Una <mark style="color:blue;">`directiva de block`</mark> tiene la misma estructura que una simple, pero en lugar de <mark style="color:blue;">`;`</mark> termina con un conjunto adicional de instrucciones entre llaves <mark style="color:blue;">`{ }`</mark>.  Si una directiva de bloque contiene otras directivas dentro de llaves, se denomina contexto.

Se considera que están dentro del contexto principal aquellas directivas colocadas en el archivo de configuración fuera de cualquier contexto.

Los eventos y las directivas http residen en el contexto principal, el servidor en http y la ubicación en el servidor.

### &#x20;Archivos de configuración

Los archivos de configuración de Nginx se encuentran en el directorio:&#x20;

```
/etc/nginx
```

y el archivo de configuración global es:&#x20;

```
/etc/nginx/nginx.conf
```

La estructura de configuración en el archivo de configuración principal

Tener en cuenta que, en Ubuntu existe una directiva de inclusión adicional:

```
include/etc/nginx/sites-enabled/*;
```

porque el directorio:

<mark style="color:blue;">`/etc/nginx/sites-enabled/`</mark> almacena los enlaces simbólicos a los archivos de configuración de sitios web creados en:

<mark style="color:blue;">`/etc/nginx/sites-available/`</mark> para habilitar los sitios.

Y el hecho de eliminar un enlace simbólico deshabilita ese sitio en particular.

### Archivo /sites-available/default

Este sería el bloque del servidor nginx predeterminado de muestra ubicado en

/etc/nginx/conf.d/default.conf en el sistema de prueba.

&#x20;

&#x20;

&#x20;

Donde:

listen: especifica el puerto en el que escucha el servidor.\
server\_name: define el nombre del servidor, que puede ser nombres exactos, nombres comodín o expresiones regulares.\
root: especifica el directorio desde el cual Nginx servirá páginas web y otros documentos.\
index: especifica los tipos de archivos de índice que se publicarán.\
location: se utiliza para procesar solicitudes de archivos y carpetas específicos.

Como tenemos instalado el nginx, si escribimos: localhost en el navegador veremos su página por def ecto. Esto es:

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

## A la hora de servir contenido estático

Una tarea importante del servidor web es entregar archivos del tipo imágenes o páginas HTML estáticas. Vamos a implementar un ejemplo en el que, según la solicitud, los archivos se entregarán desde diferentes directorios locales:

/var/www con archivos HTML y /var/www/images que contiene imágenes.

Para ello tendremos que editar el archivo de configuración y la configuración de un bloque de servidor dentro del bloque http con dos bloques de ubicación.

Crear el directorio /var/www/kirby.com y colocamos un archivo index.html con cualquier contenido de texto

Creamos el directorio /var/www/kirby.com/img y guardamos una imagen.

En el archivo de configuración predeterminado “default” ya se incluyen varios ejemplos del bloque del servidor, en su mayoría comentados, sin embargo, creemos un nuevo bloque de servidor:

&#x20;

http {

&#x20;   servidor {

&#x20;   }

}

Generalmente, el archivo de configuración puede incluir varios bloques de servidores que se distinguen por los puertos en los que escuchan y por los nombres de los servidores. Una vez que Nginx decide qué servidor procesa una solicitud, prueba el URI especificado en el encabezado de la solicitud con los parámetros de las directivas de ubicación definidas dentro del bloque del servidor.

Agregue el siguiente bloque de ubicación al bloque del servidor:

El resto de una línea después del signo # se considera un comentario.\
\


1\.      Los archivos de configuración de Nginx son:

A tener en cuenta:

\-         sites-available

\-         sites-enable

\-         conf.d\
\


2\.      He creado dos ficheros en /etc/nginx/sites-available:

2.1.    example1.conf

2.2.    example2.conf

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

3\.      Veamos el contenido de example1.com.conf

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

4\.      El contenido de punky.com.conf

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

5\.      Los dos archivos de configuración son:

Los dos sitios web se encuentran en:

/var/www/example1.com y /var/www/example2.com

Y dentro de /var/www/example1.com:

&#x20;

### REsultados

Consigo visualizar las dos páginas web desde un equipo cliente (en mi caso Ubuntu Desktop por red interna) utilizando la IP pero no el dominio, esto es:

[http://192.168.6.100:8080](http://192.168.6.100:8080) (café)

[http://192.168.6.100:8081](http://192.168.6.100:8081) (papillon)

Sin embargo, cuando utilizo el dominio del servidor: [http://haven.haven.local:8080](http://haven.haven.local:8080)  me muestra la siguiente pantalla y no me deja continuar.

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

Si quiero verificar los puertos abiertos en el servidor podemos hacer:

sudo netstat –tlpn | grep nginx

y nos muestra lo siguiente:

&#x20;

&#x20;

&#x20;

&#x20;

&#x20;

## Acceso personalizado

En lugar de utilizar los archivos de registro predeterminados, podemos definir archivos de registro personalizados para diferentes sitios web.

Para restringir el acceso a tu sitio web/aplicación o algunas partes del mismo, podemos configurar la autenticación HTTP básica. Esto se puede utilizar esencialmente para restringir el acceso a todo el servidor HTTP, bloques de servidores individuales o bloques de ubicación.

```
Comencemos por crear un archivo que almacenará nuestras credenciales de acceso (nombre de usuario/contraseña) mediante la utilidad htpasswd. Para ello instalamos apache2-utils:
```

```
sudo apt install apache2-utils
```

Como ejemplo, agreguemos el administrador de usuarios a esta lista. Tened en consideración que podemos agregar tantos usuarios como necesitemos. Las opciones son:

\-c  especificar el archivo de contraseña\
\-B  cifrar la contraseña.

Una vez que presione Enter, se te pedirá ingresar la contraseña del usuario:

$ sudo htpasswd -Bc /etc/nginx/conf.d/.htpasswd kirby

&#x20;

Luego, asignemos los permisos y la propiedad adecuados al archivo de contraseña (reemplace el usuario y grupo nginx con www-data en Ubuntu).

$ sudo chmod 640 /etc/nginx/conf.d/.htpasswd

&#x20;

&#x20;

$ sudo chown nginx:nginx /etc/nginx/conf.d/.htpasswd

&#x20;

Se puede restringir el acceso al servidor web, un solo sitio web (usando su bloque de servidor) o un directorio o archivo específico. Se pueden usar dos directivas útiles para lograr esto:

\-        auth\_basic: activa la validación del nombre de usuario y la contraseña mediante el protocolo "Autenticación básica HTTP".

\-        auth\_basic\_user\_file: especifica el archivo de la credencial.

Veamos cómo proteger con contraseña el directorio/var/www/html/protected. Para ello agregamos el bloque location /protected en el archivo de configuración “default”:

server {

&#x20;   listen         80 default\_server;

&#x20;   server\_name    localhost;

&#x20;   root           /var/www/html/;

&#x20;   index          index.html;

&#x20;   location / {

&#x20;               try\_files $uri $uri/ =404;

&#x20;       }

&#x20;  &#x20;

&#x20;   location /protected/ {

&#x20;       auth\_basic              "Restricted Access!";

&#x20;       auth\_basic\_user\_file    /etc/nginx/conf.d/.htpasswd;

&#x20;   }

}

Ahora, guardamos los cambios y reiniciamos el servicio Nginx.

$ sudo systemctl restart nginx

&#x20;

Esto significa que la próxima vez que intentes acceder a ese directorio http:// localhost/protected te pedirá que ingreses las credenciales de inicio de sesión (en mi caso user: kirby y password: la elegida).

Un inicio de sesión exitoso le permite acceder al contenido del directorio; de lo contrario, obtendrá un error de "Se requiere autorización 401".

&#x20;

Debemos crear algún contenido dentro de este directorio protected para testear correctamente el sitio. Algo que no hemos hecho.
