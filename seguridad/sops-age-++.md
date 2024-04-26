---
description: Oriol Tovar y Fernando Condori
---

# SOPS/AGE ++

## ¿Qué es SOPS y AGE?&#x20;

SOPS es un editor de archivos cifrado que admite formatos del tipo YAML, JSON, ENV, INI y BINARY. Además de soportar el cifrado de estos mediante los cifrados AWS KMS, GCP KMS, Azure Key Vault y PGP.&#x20;

Para almacenar secretos de forma segura en un repositorio de Git público o privado, puede usar la SOPS CLI de Mozilla para cifrar con OpenPGP, AWS KMS, GCP KMS y Azure Key Vault.

Los secrets en texto plano que podemos ver en el control de código fuente (por ejemplo en Github) resulta ser uno de los errores de seguridad más comunes que cometen los desarrolladores.

Por otra parte, AGE es una herramienta de encriptación simple, moderna y segura (y una biblioteca Go) con pequeñas claves explícitas, sin opciones de configuración estilo UNIX. Un archivo age se compone de dos partes:&#x20;

* un encabezado de texto que contiene la clave del archivo y&#x20;
* una carga útil binaria cifrada con él.&#x20;

En general, los archivos age deben tratarse como binarios y no son maleables sin el conocimiento de la clave del archivo.

**¿Cómo funciona?** La idea es simple, supongamos que tenemos un fichero con la información de configuración de una aplicación.  Con este fichero en texto plano y con SOPS, generaremos un nuevo fichero con los valores cifrados.&#x20;

## SOPS

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

## AGE - herramienta de cifrado

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

Lo primero que vamos a hacer es crear un directorio llamado SOPS y crear una nueva key que vamos a exportar.&#x20;

<pre><code><strong>mkdir sops
</strong></code></pre>

Creamos una nueva key:

```
age-keygen -o age-key-a.txt
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>La clave pública creada</p></figcaption></figure>

Si hacemos un cat del archivo creado con el par de claves se nos muestra algo como lo siguiente:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Archivo age-key-a.txt</p></figcaption></figure>

### Creamos un texto cifrado

```
sops ejemplo.yaml
```

Pegamos el texto en el yaml

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
cat ejemplo.yaml
```

Guardamos el archivo y ahora podremos visualizar el el contenido del fichero tal cual.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>fichero sin cifrar</p></figcaption></figure>

Para testear el uso de sops, vamos a cifrar dicho fichero. Para ello hacemos lo siguiente:

```
ops --encrypt --age age109jn5futkc5ppakuluh0qwdcfqqvgupysnyxpjggg0p0msjxnpqqrn9sgr --encrypted-regex '^(data|stringData)$' --in-place ejemplo.yaml
```

Con esta línea de comando le estamos diciendo que cifre el archivo `ejemplo.yaml` utilizando la clave pública `age109jn5futkc5ppakuluh0qwdcfqqvgupysnyxpjggg0p0msjxnpqqrn9sgr` de `age`, y solo cifrará las secciones del archivo YAML que tengan claves `data` o `stringData`. El archivo `ejemplo.yaml` será modificado directamente con los datos cifrados según los parámetros especificados.

Si ahora volvemos a probar a mirar dentro del fichero secret.yaml veremos algo como lo siguiente:&#x20;

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>Archivo ejemplo.yaml  cifrado</p></figcaption></figure>





sudo snap install helm3&#x20;
