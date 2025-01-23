# Acceso y password

Utilizar contraseñas débiles sigue siendo la mayor preocupación de seguridad de las organizaciones. Los dispositivos de red, incluyendo los routers domésticos, siempre deben tener contraseñas configuradas para limitar el acceso administrativo. Nunca dejemos la contraseña por defecto en nuestro router en casa!

En Cisco IOS podemos utilizar contraseñas en modo jerárquico y permitir diferentes privilegios de acceso al dispositivo.

A todos los switches o routers se les debe limitar el acceso administrativo asegurando:&#x20;

* EXEC privilegiado
* EXEC de usuario y, el
* Acceso Telnet remoto con contraseñas.&#x20;

Además, todas las contraseñas deben estar cifradas así como mostrar notificaciones legales.

Al elegir contraseñas, debemos usar contraseñas seguras que no sean fáciles de adivinar.&#x20;

Algunos aspectos a considerar al seleccionar una contraseña:

* Tener más de ocho caracteres de longitud.
* Usar una combinación de letras mayúsculas y minúsculas, números, caracteres especiales o secuencias numéricas.
* Evitar el uso de la misma contraseña para todos los dispositivos.
* No usar palabras comunes porque se adivinan fácilmente.

### Modos de subconfiguración

Existen diversos tipos de modos de subconfiguración. Si queremos acceder al modo de subconfiguración de línea, debemos utilizar el comando **line** seguido del tipo de línea de administración y el número al que deseamos acceder.&#x20;

Para salir de un modo de subconfiguración y volver al modo de configuración global usamos el comando **exit.**

Para pasar de cualquier modo de subconfiguración del modo de configuración global a un nivel por encima en la jerarquía de modos, escribimos el comando **exit**.

Para pasar de cualquier modo de subconfiguración al modo **EXEC** privilegiado, ingresamos el comando **end** o escribimos la combinación de teclas. **Ctrl+Z**.

## Contraseña para acceder por la línea de consola, terminales virtuales y conexión auxiliar



Cuando nos conectamos a un dispositivo, nos  encontramos en modo EXEC de usuario, que está protegido usando la consola.

Para proteger el acceso al modo EXEC del usuario:&#x20;

En el comando de configuración **line console 0** global el cero representa la primera y en la única) interfaz de consola.&#x20;

```
enable
configure terminal
hostname R1
line console 0
password cisco
login
exit
```

Para tener acceso como administrador a todos los comandos del sistema, incluida la configuración del dispositivo, debemos obtener acceso en modo **EXEC privilegiado**. Recordemos que es el método de acceso más importante porque proporciona acceso completo al dispositivo.

Para asegurar el acceso privilegiado a EXEC, usamos el comando **enable secret** _password_ global config, como se muestra en el ejemplo.

```
enable
configure terminal
SW1(config)# enable secret class
SW1(config)# exit
SW1# 
```

### Activar contraseña para acceder por  terminales virtuales

Las líneas de terminal virtual (VTY) permiten el acceso remoto utilizando **Telnet** o **SSH** al dispositivo.&#x20;

Muchos switches de Cisco admiten hasta 16 líneas VTY, de 0 al 15.

Para poder proteger las VTY, debemos acceder al modo VTY de línea y después, especificar la contraseña de VTY. Por último, habilitar el acceso a VTY con el comando **login.**

```
enable
configure terminal
line vty 0 15
password cisco
login
end
```

### Activar contraseña para acceder por la conexión auxiliar

```
line aux 0
password cisco
login
exit
```

Lo malo de esto es que la contraseña aparece en texto plano, sin cifrar. Lo podemos ver haciendo

```
show running-config
```

Por tanto, lo ideal sería cifrar todas las contraseñas. Vamos allá:

```
service password-encryption
```

Ahora si aparecen cifradas todas las contraseñas.

## Activar contraseña en el modo EXEC privilegiado

Si queremos securizar el acceso al modo EXEC de usuario cn privilegios, podemos hacer.

```
configure terminal
enable password cisco 
enable secret class
```

### Cifrando las contraseñas

Los archivos `startup-config` y `running-config` muestran la mayoría de las contraseñas en texto plano y está claro que es una amenaza de seguridad flagrante porque cualquiera puede descubrir las contraseñas si tiene acceso a estos archivos.

Para cifrar las contraseñas de texto sin formato, utilizamos el comando **service password-encryption** global config como se muestra en el ejemplo.

<pre><code><strong>SW1# configure terminal
</strong><strong>SW1(config)# service password-encryption
</strong><strong>SW1(config)#
</strong></code></pre>

El comando aplica un cifrado **débil** (MD5) a todas las contraseñas no cifradas. Esta encriptación solo se aplica a las contraseñas del archivo de configuración; no a las contraseñas mientras se envían a través de los medios.&#x20;

El propósito de este comando es evitar que individuos no autorizados vean las contraseñas en el archivo de configuración.

## Mensajes de aviso

Pedir contraseñas de acceso a los dispositivos es una forma de mantener al personal no autorizado fuera de la red, sin embargo es vital proporcionar un método para declarar que solo el personal autorizado debe intentar acceder al dispositivo.&#x20;

Para ello debemos crear un aviso que muestre por consola  un mensaje.&#x20;

Nota: Los avisos pueden ser una parte importante en los procesos legales en el caso de una demanda por el ingreso no autorizado a un dispositivo. Algunos sistemas legales no permiten la acusación, y ni siquiera el monitoreo de los usuarios, a menos que haya una notificación visible.

```
SW1# enable
SW1# configure terminal
SW1(config)# banner motd #Authorized Access Only#
SW1#(config)# end
```

