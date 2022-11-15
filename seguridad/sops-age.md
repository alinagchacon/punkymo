---
description: N-Tech Admin Group
---

# SOPS/AGE



En [https://github.com/mozilla/sops](https://github.com/mozilla/sops) nos descargamos la versión estable. Aquí [https://github.com/mozilla/sops/releases:](https://github.com/mozilla/sops/releases)

1\.      Copiamos la URL de la versión para Debian.

2\.      Descargamos la herramienta en el terminal de Ubuntu o Debian.&#x20;

Entonces:

<pre><code>wget https://github.com/mozilla/sops/releases/download/v3.7.3/sops_3.7.3_amd64.deb
<strong>sudo dpkg –i ./sops_3.7.3_amd64.deb
</strong>su –
dpkg –i sops_3.7.3_amd64.deb
rm sops_3.7.3_amd64.deb</code></pre>

Para verificar la versión instalada:

```
sops -v
```

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

### Age

Ahora vamos a descargar e instalar age desde [https://github.com/FiloSottile/age](https://github.com/FiloSottile/age)

Y hacemos la misma operación con wget:

<figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

```
wget –O age.tar.gz 
https://github.com/FiloSottile/age/releases/download/v1.0.0.0/age-v1.0.0-linux-amd64.tar.gz
```

Una vez instalado podemos hacer <mark style="color:blue;">`ls –l`</mark> para comprobar el estado de los ficheros y nos disponemos a descomprimir el fichero que hemos descargado:

```
tar xf age.tar.gz
```

Vamos a mover los archivos binarios **age** y el generador de claves **age-keygen** a la carpeta /usr/local/bin

```
mv /age/age /usr/local/bin/
mv /age/age-keygen /usr/local/bin/
```

Y eliminamos el directorio de los archivos que ya no son necesarios:

```
rm –r-f age
rm age.tar.gz
```

Para mostrar la versión correspondiente**:**

```
age –version
age-keygen -version
```

Puedes hacer ver la ayuda:

```
age – h o age –help
```

### Creando una clave pública y privada

&#x20;Para encriptar necesitamos primero crear una clave pública y privada, con lo cual

```
age-keygen –o key.txt
```

<figure><img src="../.gitbook/assets/image (222).png" alt=""><figcaption></figcaption></figure>

La clave privada no se debe compartir con nadie, como su nombre indica, es privada.&#x20;

De todas maneras, si hacemos cat key.txt o more key.txt podemos ver nuestra clave pública y privada generadas:

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption><p>clave pública y privada </p></figcaption></figure>

Movemos nuestra recién generada clave a un directorio seguro:

```
mkdir ~/.sops
mv age/key.txt ~/.sops
```

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption><p>Tener en cuenta que estoy como root</p></figcaption></figure>

Tengamos en cuenta que al mover nuestro fichero de claves pública y privada a nueva localización necesitamos agregar variables de entorno. Para ello, nos aseguramos que estemos un ejecutando zsh (Z shell) o bashrc  (bash) o bench RC. Como estoy testeando esto desde Debian, pues tengo el bash file:

```
nano ~/.bashrc
```

Agregamos la siguiente línea al final del fichero:

```
export PATH="$PATH:$HOME/.local/bin"
export SOP_AGE_KEY_FILE=$HOME/.sops/key.txt  
source ~/.bashrc
```

### Encriptando un fichero

Tenemos varias opciones para encriptar un fichero como por ejemplo, desde la línea de comandos, utilizando tu propio editor de textos.&#x20;

Vamos a comenzar con un ejemplo donde utilizamos el lenguaje de marcado YAML para lo que tendremos que editar algunos ficheros yaml.&#x20;

```
nano secret.yaml
```

Y editamos el fichero:

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

Por supuesto que si hacemos:

```
cat secret.yaml
```

Veremos el contenido del fichero tal cual. Por tanto, vamos a cifrarlo. Para ello hacemos lo siguiente:

```
sops --encrypt --age $(cat $SOPS_AGE_KEY_FILE |grep -oP "public key: \K(.*)") --encrypted-regex '^(data|stringData)$' --in-place ./home/pepe/secret.yaml 
```

Con esta línea de comando le estamos diciendo que use age con el formato de cifrado y que tome la variable llamada Sops y que obtenga el archivo de clave age de la variable Sops y lo tome para la clave pública para lo cual debe utilizar la expresión regular cifrada y buscar los datos de la cadena en la clave que acabamos de mencionar y lo vamos a cifrar en su lugar y pasar en el archivar.

Si ahora volvemos a probar a mirar dentro del fichero secret.yaml veremos algo como lo siguiente:

<figure><img src="../.gitbook/assets/image (223).png" alt=""><figcaption><p>secret.yaml cifrado</p></figcaption></figure>

Donde podemos ver que el usuario y la contraseña están cifrados (AES256).

### Descifrando el archivo

Para descifrar el archivo utilizamos el mismo comando pero utilizando la opción <mark style="color:blue;">`decryp.`</mark>

<pre><code><strong>sops --decrypt --age $(cat $SOPS_AGE_KEY_FILE |grep -oP "public key: \K(.*)") --encrypted-regex '^(data|stringData)$' --in-place ./home/pepe/secret.yaml</strong></code></pre>

Y si volvemos a comprobar con <mark style="color:blue;">`cat secret.yaml`</mark> veremos que está exactamente igual.

Pero qué pasa si queremos cifrar este archivo y aplicarlo directamente en la aplicación que lo utiliza?. Para ello, volvemos a cifrar el archivo con el mismo comando anterior pero con una ligera diferencia: le decimos que lo descifre para la aplicación que lo utiliza que en este ejemplo sería Kubernete:

```
sops --decrypt --age $(cat $SOPS_AGE_KEY_FILE |grep -oP "public key: \K(.*)") --encrypted-regex '^(data|stringData)$' --in-place ./home/pepe/secret.yaml | kubectl apply -f -
```

Al ejecutar este comando para Kubernete nos crea un fichero con la clave:

<mark style="color:blue;">`secret/mysql-secret created`</mark>

Podemos  comprobar el uso de kubernete haciendo:

```
kubectl describe secrets mysql-secret
```

Esto nos muestra el fichero yaml pero en el lugar de usuario y contraseña nos diría algo como:

<mark style="color:blue;">`MYSQL_USER: 9 bytes`</mark>

<mark style="color:blue;">`MYSQL_PASSWORD: 25 bytes`</mark>

Si queremos ver el usuario y contraseña entonces haríamos:

<pre><code><strong>kubectl get secret mysql-secret-test -o jsonpath?'{.data}'</strong></code></pre>

y nos muestra algo como lo siguiente:

<figure><img src="../.gitbook/assets/image (85).png" alt=""><figcaption><p>Con esto </p></figcaption></figure>

Con esto ya tenemos el usuario y contraseña para el sistema:

<figure><img src="../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

<pre><code><strong>kubectl get secret mysql-secret-test -o jsonpath?'{.data.MYSQL_PASSWORD}' | base64 --decode</strong></code></pre>



Lo mismo si queremos ver el usuario:

<pre><code><strong>kubectl get secret mysql-secret-test -o jsonpath?'{.data.MYSQL_USER}' | base64 --decode</strong></code></pre>



<mark style="color:red;">To be continued ...</mark>

### Links

* Aprendiendo de:\
  [https://www.youtube.com/watch?v=1BquzE3Yb4I](https://www.youtube.com/watch?v=1BquzE3Yb4I)&#x20;
* [https://blog.gitguardian.com/a-comprehensive-guide-to-sops/](https://blog.gitguardian.com/a-comprehensive-guide-to-sops/)

&#x20;
