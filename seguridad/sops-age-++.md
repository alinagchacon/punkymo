---
description: Oriol Tovar y Fernando Condori
---

# SOPS/AGE ++

## ¿Qué es SOPS y AGE?&#x20;

SOPS es un editor de archivos cifrado que admite formatos del tipo YAML, JSON, ENV, INI y BINARY. Además de soportar el cifrado de estos mediante los cifrados AWS KMS, GCP KMS, Azure Key Vault y PGP.&#x20;

Para almacenar secretos de forma segura en un repositorio de Git público o privado, puede usar la SOPS CLI de Mozilla para cifrar con OpenPGP, AWS KMS, GCP KMS y Azure Key Vault.

Los secrets en texto plano que podemos ver en el control de código fuente (por ejemplo en Github) resulta ser uno de los errores de seguridad más comunes que cometen los desarrolladores.



## AGE&#x20;

Trata de una herramienta de encriptación simple, moderna y segura (y una biblioteca Go) con pequeñas claves explícitas, sin opciones de configuración estilo UNIX. Un archivo age se compone de dos partes:&#x20;

* un encabezado de texto que contiene la clave del archivo y&#x20;
* una carga útil binaria cifrada con él.&#x20;

En general, los archivos age deben tratarse como binarios y no son maleables sin el conocimiento de la clave del archivo.

## SOPS

Para la instalación de sops no tendremos ninguna dificultad, seguiremos los siguientes pasos.&#x20;

Lo primero sería descargar el paquete correspondiente:&#x20;

```
wget https://github.com/mozilla/sops/releases/download/v3.7.3/sops_3.7.3_amd64.deb
```

Y una vez descargado, lo instalamos:

```
sudo dpkg -i sops_3.7.3_amd64.deb
```

Una vez instalada podemos ver la versión de SOPS:

```
sops --version
sops -v
```

## AGE

Nos descargamos AGE mediante wget:&#x20;

```
sudo wget https://github.com/FiloSottile/age/releases/download/v1.0.0-rc.1/age-v1.0.0-rc.1-linux-amd64.tar.gz
```

Una vez lo tengamos descargado, podemos descomprimir el archivo con el comando tar. Esto nos crea una carpeta, así que nos movemos dentro de la misma:

```
 tar -xvf age-v1.0.0-rc.1-linux-amd64.tar.gz
 cd age
 sudo cp age* /usr/local/bin/
```

Una vez instalada y descomprimida movemos los archivos age y age-key a usr/local/bin:&#x20;

```
mv /age/age /usr/local/bin/ 
mv /age/age-keygen /usr/local/bin/
```

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="319"><figcaption><p>Archivos age y age-keygen</p></figcaption></figure>

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
age-keygen > keyname.txt
```

Y le cambiamos los permisos:

<pre><code><strong>chmod 600 keyname.txt
</strong></code></pre>

Exportamos las variables usando la key

```
export SOPS_AGE_RECIPIENTS=(public key)
```

Ejemplo de exportar `vars` usando la key

<pre><code><strong>export SOPS_AGE_RECIPIENTS=age1gqv7p4gdy4ffwux4epaxeu905
</strong></code></pre>

```
export SOPS_AGE_KEY_FILE=~sops/keyname.txt
```

### Creamos ahora un texto encriptado

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

Guardamos el archivo y ahora ya podremos visualizar el archivo con el comando cat:

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Visualizando el archivo creado</p></figcaption></figure>

