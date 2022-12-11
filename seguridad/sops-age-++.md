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

Para la instalación de sops no tendremos ninguna dificultad, seguiremos los siguientes pasos: el primer paso es usar los siguientes comandos: wget&#x20;

<mark style="color:blue;">`https://github.com/mozilla/sops/releases/download/v3.7.3/sops_3.7.3_amd64.deb`</mark>

```
wget https://github.com/mozilla/sops/releases/download/v3.7.3/sops_3.7.3_amd64.deb
```

```
sudo dpkg -i sops_3.7.3_amd64.deb
```

Una vez instalada podemos ver la versión de SOPS:

```
sops --version
sops -v
```

## AGE

Instalamos AGE mediante wget:&#x20;

```
wget [https://github.com/FiloSottile/age/releases/download/v1.0.0-rc.1/age-v1.0.0-rc.1-linux-amd64.tar.gz] (https://github.com/FiloSottile/age/releases/download/v1.0.0-rc.1/age-v1.0.0-rc.1-linux-amd64.tar.gz)
```

```
 tar -xvf age-v1.0.0-rc.1-linux-amd64.tar.gz
```

```
cd age
```

```
cp age* /usr/loca/bin/
```

```
age --version
```



### Trabajando con SOP

<pre><code><strong>mkdir sops
</strong></code></pre>

Creamos una nueva key:

<pre><code><strong>chmod 600 keyname.txt
</strong></code></pre>

Exportamos las variables usando la key

```
export SOPS_AGE_RECIPIENTS=(public key)
```

Ejemplo de exportar vars usando la key

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

<mark style="color:red;">to be continued ...</mark>
