# SFTP

### ¿Qué es SFTP?

SFTP es el protocolo FTP pero utliza SSH como canal seguro. El nombre viene de las siglas: SSH File Transfer Protocol y es una extensión de Secure Shell Protocol (SSH)  para poder hacer transmisiones de archivos.

SFTP nos proporciona seguridad, algo que es importante para la transferencia de archivos porque de lo contrario, los ficheros se transmiten de modo inseguro por la red, como ya hemos podido observar utlizando Wireshark. Por lo tanto, SFTP aporta mayor seguridad a las transmisiones, y  es más recomendable su uso.&#x20;

### Requisitos

Voy a configurar mi servidor SFTP, en la misma VM donde ya tengo FTP, esto es: Un servidor Ubuntu Server 24.04.2 LTS.

### Configurando el servidor SFTP

Lo primero de todo es asegurarnos de tener el servicio openssh instalado, para ello y como procedimiento general podemos hacer la instalación del paquete  OpenSSH si aún no está instalado.

```
sudo apt update | sudo apt upgrade
sudo apt install openssh-server
```

#### Creamos un grupo y un usuario SFTP

Lo interesante es poder crear usuarios que no radiquen en el sistema. Para ello, creamo un grupo explícitamente para usuarios del servicio SFTP y agregamos un usuario a este grupo. De este modo podemos administrar los permisos de una manera más eficaz.

**Creamos el grupo sftp\_users:**

```
sudo groupadd sftp_users
```

**Configuramos un usuario que he llamado Bonifacio:**

```
sudo useradd -m -g sftp_users -s /sbin/nologin bonifacio
```

<kbd>**Establecemos**</kbd>**&#x20;una contraseña para dicho usuario:**

```
sudo passwd bonifacio
```

Y pasamos a configurar la estructura del directorio a donde tendrá acceso dicho usuario cuando haga el ftp.

#### Configurando la estructura del directorio

Vamos a crear los directorios donde los usuarios del servicio SFTP van a subir sus archivos. Por supuesto, con los permisos adecuados.

**Creamos el directorio principal:**

```
sudo mkdir -p /dataftp/bonifacio/upload
```

**Establecemos los propietarios y permisos correspondientes**

```
sudo chown root:sftp_users /dataftp/bonifacio 
sudo chmod 755 /dataftp/bonifacio
sudo chown bonifacio:sftp_users /dataftp/bonifacio/upload
```

#### Configurando el demonio SSH para SFTP

Ahora, debemos configurar el demonio SSH para restringir a los usuarios a sus directorios de inicio y habilitar la funcionalidad SFTP. Para ello nos vamos al archivo de configuración de SSH:

```
sudo nano /etc/ssh/sshd_config
```

Y al final del archivo, añadimos la siguiente configuración:

```
Match Group sftp_users 
ChrootDirectory /data/%u 
ForceCommand internal-sftp 
AllowTcpForwarding no 
X11Forwarding no
```

#### Reiniciando el servicio SSH

Finalmente, para que los cambios surtan efecto, tenemos que reiniciar el servicio SSH, por tanto:

```
sudo systemctl restart sshd
```

#### Probando la configuración SFTP

Solo nos queda probar el servicio. En mi caso, desde un terminal de Linux me he conectado al servidor en la VM, esto es:

```
sftp bonifacio@mi_IP
```

#### Algunos comandos básicos para utilizar SFTP

Una vez conectados a través de SFTP, podemos utilizar algunos comandos para administrar archivos como:

Para ayuda:

```
help
```

Para subir un archivo al servidor:

```
put file.txt
```

Para descargar un archivo del servidor:

```
get file.txt
```

Si queremos listar los archivos en el directorio actual del servidor:

```
ls
```

Para cambiar el directorio en el servidor:

```
cd directorio_remoto
```

Si queremos listar el contenido del directorio local:

```
lls
```

&#x20;

### Algo extra ...

1. ¿Qué hacen las opciones que añadimos al final del archivo sshd\_config? ¿por qué son importantes?
2. ¿Qué significa exportar las X en el entorno de SSH?
3. Prueba la conexión de SFTP y monitoriza el servicio con Wireshark. Haz lo mismo con FTP.

