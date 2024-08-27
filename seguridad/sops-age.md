---
description: Gracias a Xavier Adell, Joel González, Oriol Tovar y Fernando Condori
---

# SOPS/AGE

Un inconveniente que podemos encontrarnos cuando tenemos nuestras aplicaciones en Git es el tema de las contraseñas que pueden quedar al acceso de todos. ¿Cómo hacer  que nuestros "secretos" estén lo más seguros posible?&#x20;

Aquí traigo una herramienta muy interesante que nos permite mantener a salvo nuestros "secretos", se trata de [Mozilla SOPS](https://github.com/mozilla/sops). Esta herramienta nos permite cifrar y descifrar archivos con soporte para YAML, JSON, .ENV e incluso binarios, y además se integra fácilmente con diferentes [KMS](https://simple.wikipedia.org/wiki/Key\_Management\_Service).&#x20;

## ¿Qué es SOPS y qué es AGE?&#x20;

Mozilla SOPS (**S**ecrets **OP**eration**S - SOPS**) es un editor de archivos cifrado que admite formatos del tipo YAML, JSON, ENV, INI y BINARY. Además de soportar el cifrado de estos mediante los cifrados AWS KMS, GCP KMS, Azure Key Vault y [PGP](https://es.wikipedia.org/wiki/Pretty\_Good\_Privacy).&#x20;

Para almacenar secretos de forma segura en un repositorio de Git público o privado, puede usar SOPS  para cifrar con [OpenPGP](https://www.openpgp.org), AWS KMS, GCP KMS y Azure Key Vault.

Los secrets en texto plano que podemos ver en el control de código fuente (por ejemplo en Github) resulta ser uno de los errores de seguridad más comunes que cometen los desarrolladores.

Sops tiene características que lo hacen interesante. Por ejemplo:

* Se integra con almacenes de claves (keystores) gestionados en la nube, como AWS y GCP Key Management Service (KMS) o Azure Key Vault, como fuentes de claves de cifrado.&#x20;
* Funciona en varias plataformas y es compatible con las PGP keys, lo que permite un control de acceso detallado a los secrets de archivo por archivo.&#x20;
* Deja la key de identificación en texto plano para que los secrets puedan seguir siendo localizados y difundidos por git.&#x20;

Por otra parte, `AGE` es una herramienta de encriptación simple, moderna y segura (y una biblioteca Go) con pequeñas claves explícitas, sin opciones de configuración estilo UNIX. Un archivo age se compone de dos partes:&#x20;

* un encabezado de texto que contiene la clave del archivo y&#x20;
* una carga útil binaria cifrada con él.&#x20;

En general, los archivos age deben tratarse como binarios y no son maleables sin el conocimiento de la clave del archivo.

**¿Cómo funciona?** La idea es simple, supongamos que tenemos un fichero con la información de configuración de una aplicación.  Con este fichero en texto plano y con SOPS, generaremos un nuevo fichero con los valores cifrados.&#x20;

## Instalando SOPS

Comencemos por la instalación de sops que no conlleva ninguna dificultad.  Lo primero sería descargar el paquete correspondiente:&#x20;

```
sudo curl --silent --location --remote-name https://github.com/getsops/sops/releases/download/v3.8.1/sops-v3.8.1.linux.amd64
```

donde:

**--silent:** Es una opción hace que `curl` funcione en modo silencioso, por lo que no muestra el progreso ni mensajes informativos.&#x20;

**--location:** Habilita el seguimiento automático de redirecciones HTTP. Si la URL especificada devuelve una redirección, curl seguirá automáticamente la redirección hasta que alcance la ubicación final del recurso.

**--remote-name:** Es una opción que indica a curl que debe utilizar el nombre de archivo final de la URL para guardar el archivo descargado en el directorio actual. Válido si quieres que `curl` use el nombre de archivo por defecto en lugar de especificar uno. Esto implica que, el fichero a descargar se guardará con el mismo nombre que tiene en la URL.

Una vez descargado, lo instalamos:

```
sudo install --owner root --group root --mode 555 sops-v3.8.1.linux.amd64 /usr/local/bin/sops
```

La opción  `--mode 555` establece los permisos del archivo copiado como `555` (modo octal). Esto significa que el propietario, el grupo y y el resto de usuarios solo tienen permisos de lectura y ejecución.&#x20;

Una vez instalada podemos ver la versión de SOPS:

```
sops --version
sops -v
```

## Instalando AGE

AGE es una utilidad de cifrado simple, moderna y segura. Nos descargamos la herramienta mediante curl en modo silencioso:&#x20;

```
sudo curl --silent --location --remote-name https://github.com/FiloSottile/age/releases/download/v1.1.1/age-v1.1.1-linux-amd64.tar.gz
```

Una vez lo tengamos descargado, podemos descomprimir el archivo con el comando tar. Esto nos crea una carpeta, así que nos movemos dentro de la misma:

```
sudo tar --extract --gzip --file age-v1.1.1-linux-amd64.tar.gz --strip-components 1 --directory /usr/local/bin  age/age age/age-keygen
```

donde:

La opción **--strip-components 1** le indica a `tar` que debe eliminar el primer nivel de directorio al extraer los archivos. De este modo, solo extrae  los archivos contenidos en el directorio raíz.

La opción **--directory /usr/local/bin** especifica el directorio de destino donde se tienen que copiar los archivos a extraer. En nuestro caso, los archivos extraídos se copian en el directorio `/usr/local/bin`.

Finalmente, **age/age** y **age/age-keygen** son los archivos o directorios que se van a extraer y copiar en el directorio `/usr/local/bin`.

Al igual que con SOP, podemos ver la versión del paquete instalado:

```
 age --version
```

### Trabajando con SOP

Lo primero que vamos a hacer es crear un directorio llamado `.sops` donde almacenaremos el par de llaves pública y privada que vamos a  crear y exportar.&#x20;

Creamos el par de claves público y privado:

```
age-keygen -o key.txt
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Generando el par de claves público y privado</p></figcaption></figure>

Si hacemos un cat del archivo creado con el par de claves se nos muestra algo como lo siguiente:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Archivo key.txt con el par de claves generadas</p></figcaption></figure>

La llave privada que se muestra en el fichero key.txt no debemos guardarla en ningún repositorio Git ni en ninguna carpeta al acceso de cualquiera. Para dar un poco de seguridad a nuestra clave es que vamos a crear la carpeta:  `~/.sops` y mover la clave generada.

<pre><code><strong>mkdir ~/.sops
</strong><strong>mv key.txt ~/.sops
</strong></code></pre>

Finalmente, para poder trabajar con comodidad exportaremos nuestro archivo con el par de claves como una variable de entorno hacia nuestro .zshrc o .bashrc. En mi caso sería .bashrc así que hago lo siguiente:

```
echo "export SOPS_AGE_KEY_FILE=$HOME/.sops/key.txt" >> ~/.bashrc
```

Una vez terminado configurado nuestro sistema, ya estaríamos en condiciones de cifrar nuestros secretos dentro de los archivos. Hay muchas formas de abordar esto dependiendo del tipo de archivo, por lo pronto, haremos una prueba a modo de ejemplo.

### Ejemplo 1 - Creamos un texto cifrado

Primeramente vamos a ver el uso de SOPS & AGE con un archivo cualquier `.txt`. Para ello creamos un archivo y lo vamos a cifrar de la siguiente manera:

```
echo "Este es un ejemplo de cifrado con SOPS & AGE ">> ejemplo.txt
cat ejemplo.txt
Este es un ejemplo de cifrado con SOPS & AGE 
```

Ahora vamos a utitlizar las herramientas configuradas para cifrar y descifrar el archivo ejemplo.txt. Usaremos sops con la clave pública que hemos exportado al .bashrc y con la opción --in-place le estaremos diciendo que cifre en el mismo archivo.

```
sops --encrypt --age $(cat $SOPS_AGE_KEY_FILE |grep -oP "public key: \K(.*)")  --in-place ./ejemplo.txt
```

Si ahora hacemos un cat ejemplo.txt veremos que el archivo es ilegible:

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Archivo ejemplo.txt cifrado con sops y age</p></figcaption></figure>

Para descifrar el archivo tendríamos que utilizar el mismo comando pero con la opción --decrypt. Veamos:

```
sops --decrypt --age $(cat $SOPS_AGE_KEY_FILE |grep -oP "public key: \K(.*)")  --in-place ./ejemplo.txt
```

Y podemos comprobar que, efectivamente, se ha descifrado el archivo:

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1).png" alt=""><figcaption><p>Archivo ejemplo.txt descifrado</p></figcaption></figure>

### Ejemplo 2 - Archivos YAML

Probemos ahora con un archivo `YAML` que vamos a crear a modo de ejemplo. Para ello escribimos algo como lo siguiente y lo almacenamos en un fichero como por ejemplo: `midocker.yml`.

```
apiVersion: v1
kind: Secret
metadata:
    name: mysql-secret
    namespace: default
stringData:
    MYSQL_USER: myuserroot
    MYSQL_PASSWORD: my_secret_password
```

Le echamos  un vistazo al archivo

```
cat midocker.yml
```

Guardamos el archivo y ahora podremos visualizar el el contenido del fichero tal cual.

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption><p>Fichero .yml sin cifrar</p></figcaption></figure>

Para testear el uso de sops, vamos a cifrar dicho fichero. Para ello hacemos lo siguiente:

```
sops --encrypt --age $(cat $SOPS_AGE_KEY_FILE |grep -oP "public key: \K(.*)") --encrypted-regex '^(data|stringData)$' --in-place ./midocker.yml 
```

Con esta línea de comando le estamos diciendo que cifre el archivo `midocker.yml` utilizando la clave pública de `age`, que hemos exportado. Como veréis  solo cifrará las secciones del archivo YAML que tengan claves `data` o `stringData`. El archivo `midocker.yml` será modificado directamente con los datos cifrados según los parámetros especificados.

Si ahora volvemos a probar a mirar dentro del fichero `midocker.yml` veremos algo como lo siguiente:&#x20;

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption><p>Archivo midocker.yml cifrado</p></figcaption></figure>

Igual que en el primer ejemplo, para descifrar el archivo volvemos a usar el mismo comando con la opción --decrypt y podremos volver a visualizar todo el contenido del archivo.

```
sops --decrypt --age $(cat $SOPS_AGE_KEY_FILE |grep -oP "public key: \K(.*)") --encrypted-regex '^(data|stringData)$' --in-place ./midocker.yml 
```

### Ejemplo 3- un archivo de docker

En este caso voy a utilizar un archivo de docker compose que hemos utilizado en otro apunte. En este caso tengo el siguiente archivo `docker-compose.yml`

```
version: "3.8"

services:
  # PHP service
  app:
    image: php:8-fpm
    container_name: miAppPHP    
    networks:
      - appnet
      
  # MySQL database service
  db:
    image: mysql:8.0
    container_name: miAppMySQL
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: 1234
    networks:
      - appnet

  # PHPMYADMIN
  phpmyadmin:
    image: phpmyadmin
    container_name: miAppPhpMyAdmin
    working_dir: /
    environment:
      PMA_ARBITRARY: 1      
    ports:
      - 8089:80
    networks:
      - appnet

  # Nginx service
  nginx:
    image: nginx
    container_name: miAppNginx
    ports:
      - 89:80

  # RED
  networks:
      - appnet

networks:
  appnet:
    driver: bridge

```

Una vez guardado el archivo como otro\_docker.yml voy a proceder a cifrar únicamente la parte que contiene la password de MYSQL, o sea, la línea: `MYSQL_ROOT_PASSWORD: 1234`.

Ciframos el archivo con el comando siguiente:

```
sops --encrypt --age $(cat $SOPS_AGE_KEY_FILE |grep -oP "public key: \K(.*)") --encrypted-regex '^(MYSQL_ROOT_PASSWORD)$' --in-place ./midocker.yml 
```

Si visualizamos el contenido se vería algo como lo siguiente:

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption><p>Archivo yml parcialmente cifrado</p></figcaption></figure>

Ahora volvemos a descifrar el archivo con el mismo comando pero esta vez con la opcion --decrypt:

```
sops --decrypt --age $(cat $SOPS_AGE_KEY_FILE |grep -oP "public key: \K(.*)") --encrypted-regex '^(MYSQL_ROOT_PASSWORD)$' --in-place ./midocker.yml 
```

Y si comprobamos, veremos el contenido en su estado original.

Como hemos dicho al inicio de este apunte, es posible utilizar estas herramientas con `Kubernetes`, con archivos `.ENV`, archivos `JSON.`

Sería cuestión de no subir los contenidos descifrados al Git para lo cual debemos agregar los arhivos   por ejemplo, .decrypted\~secretos.json, al .gitignore para que no lo suba a Git.&#x20;

### Links

* Aprendiendo de:\
  [https://www.youtube.com/watch?v=1BquzE3Yb4I](https://www.youtube.com/watch?v=1BquzE3Yb4I)&#x20;
* [https://blog.gitguardian.com/a-comprehensive-guide-to-sops/](https://blog.gitguardian.com/a-comprehensive-guide-to-sops/)
* [https://www.youtube.com/watch?v=PFLimPh5-wo ](https://www.youtube.com/watch?v=PFLimPh5-wo)
* [https://www.returngis.net/2022/01/proteger-secretos-con-mozilla-sops-y-azure-key-vault-y-descifrarlos-desde-flux-cd/#:\~:text=Existen%20varias%20herramientas%20para%20hacer,integrarse%20fácilmente%20con%20diferentes%20KMS.](https://www.returngis.net/2022/01/proteger-secretos-con-mozilla-sops-y-azure-key-vault-y-descifrarlos-desde-flux-cd/)
* [https://fluxcd.io/flux/guides/mozilla-sops/](https://fluxcd.io/flux/guides/mozilla-sops/)
* [https://sleeplessbeastie.eu/2024/03/20/how-to-utilize-sops-with-age-encryption/](https://sleeplessbeastie.eu/2024/03/20/how-to-utilize-sops-with-age-encryption/)&#x20;
* [https://www.youtube.com/watch?v=R0csVV\_y53w](https://www.youtube.com/watch?v=R0csVV\_y53w)

&#x20;

