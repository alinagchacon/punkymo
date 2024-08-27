# Anaconda & Spyder

Anaconda es una distribución de Python y R para cálculos científicos del tipo: ciencia de datos, aplicaciones de Machine Learning, procesamiento de datos a gran escala, análisis predictivo, etc. Entre sus ventajas podemos encontrar el hecho de simplificar la gestión e implementación de paquetes. Lo podemos instalar en Windows, Linux y macOS.&#x20;

Las distribuciones **Anaconda Distribution y Anaconda Individual Edition** son gratuitos pero existen otros productos de la compañía como  **Anaconda Team Edition** y **Anaconda Enterprise Edition**, que son comerciales.

Las diferentes versiones de paquetes de Anaconda se administran a través del sistema de gestión de paquetes **conda**.&#x20;

### Anaconda Navigator <a href="#que-es-anaconda-navigator" id="que-es-anaconda-navigator"></a>

Se trata de una GUI o interfaz gráfica de usuario que viene incluida en la distribución Anaconda. Dicha interfaz nos permite iniciar aplicaciones y administrar los paquetes y distintos entornos y canales conda sin usar comandos.&#x20;

El Navigator puede buscar paquetes en **anaconda.org** o en un repositorio local de Anaconda.

Por tanto, **Conda** es tanto un administrador de paquetes como un administrador de entorno. Esto nos asegura que cada versión de cada paquete tenga todas las dependencias que requiere y funcione correctamente.

### Ejecutar código con Navigator <a href="#como-puedo-ejecutar-codigo-con-navigator" id="como-puedo-ejecutar-codigo-con-navigator"></a>

Una forma es con **Spyder**. También podemos utilizar **Jupyter Notebooks,** un sistema cada vez más popular que combina código, texto descriptivo, salida, imágenes e interfaces interactivas en un solo archivo de cuaderno que se edita, visualiza y utiliza en un navegador web.

### ¿Y qué es Sypder?

Es un potente entorno de desarrollo interactivo y multiplataforma de código abierto (IDE) para Python. Incluye soporte de herramientas interactivas para la inspección de datos e incorpora controles de calidad específicos de Python. Por tanto, se trata de un IDE multiplataforma a través de Anaconda.

### Script de instalación

Descargamos el script que nos permite instalar la aplicación, pero previamente instalamos los paquetes que necesita:

```
apt-get install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6
```

Ahora descargamos el script en cuestión. Si vas a la url siguiente, podemos ver las versiones para iOS, Linux y Windows:

```
https://repo.anaconda.com/archive
```

De ese modo, nos podemos descargar la versión que necesitemos. En mi caso:

<pre><code><strong>curl -O https://repo.anaconda.com/archive/Anaconda3-2024.06-1-Linux-x86_64.sh
</strong></code></pre>

Ya podemos instalar anaconda, basta ejecutar el script:

<pre><code><strong>./Anaconda3-2024.06-1-Linux-x86_64.sh
</strong></code></pre>

Durante el proceso de instalación de Anaconda nos hace tres preguntas: le decimos que Yes a todo.

Iniciamos Anaconda:

```
source ~/.bashrc
```

Y de este modo podemos comprobar si todo ha ido correctamente:

```
conda --version
```

<figure><img src="../.gitbook/assets/image (383).png" alt="" width="563"><figcaption><p>iniciando Anaconda</p></figcaption></figure>

#### Creando un nuevo entorno para Anaconda

Es una manera de poder establecer el trabajo en varios proyectos a la vez. Para ello:

```
conda create --name miconda python=3.11.2
```

En mi caso he llamado _**miconda**_ al nuevo entorno de trabajo y le he añadido la versión de python. Una vez que termina el proceso de instalación del entorno y los paquetes correspondientes, nos aparece el siguiente mensaje:

<figure><img src="../.gitbook/assets/image (384).png" alt="" width="563"><figcaption></figcaption></figure>

Es importante tenerlo en cuenta porque si deseamos desactivar el entorno de trabajo (no es obligado) lo podemos apagar haciendo:

```
conda deactivate miconda
```

Si es necesario instalar paquetes extras, como es el caso de la librería `numpy` podemos hacer lo siguiente:

```
conda install numpy
```

{% hint style="info" %}
NumPy es una biblioteca popular de Python, de código abierto, que se usa para realizar cálculos matemáticos y científicos, útiles para proyectos de Data Science.  Su nombre proviene de: numerical python.&#x20;
{% endhint %}

### Instalar anaconda-navigator

Basta hacer lo siguiente:

```
conda install anaconda-navigator
```

### Spyder

Se puede instalar simplemente haciendo:

```
conda install spyder 
```

### Links

* [https://docs.anaconda.com/anaconda/install/linux/](https://docs.anaconda.com/anaconda/install/linux/)&#x20;
* [https://es.hostzealot.com/blog/about-vps/configuracion-de-anaconda-en-ubuntu-o-debian-guia-completa](https://es.hostzealot.com/blog/about-vps/configuracion-de-anaconda-en-ubuntu-o-debian-guia-completa)
* [https://datascientest.com/es/numpy-la-biblioteca-python](https://datascientest.com/es/numpy-la-biblioteca-python)&#x20;
* [https://docs.spyder-ide.org/current/installation.html](https://docs.spyder-ide.org/current/installation.html)
* [https://www.datahack.es/introduccion-a-anaconda-python-que-es/](https://www.datahack.es/introduccion-a-anaconda-python-que-es/)
