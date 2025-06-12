---
description: Instalando PHP
---

# PHP-FPM

Antes de poder configurar NGINX para trabajar con una aplicación en PHP tenemos que asegurarnos de instalar la versión correcta. En este caso, PHP-FPM.

PHP-FPM puede trabaja con servidores web como **Nginx** o **Apache**, que envían las solicitudes PHP,  procesan los scripts PHP y devuelven la respuesta al servidor web, quien se encarga de entregarla al usuario.&#x20;

En configuraciones con Nginx, es un imprescindible el PHP-FPM dado que Nginx no tiene soporte nativo para PHP, pero si con PHP-FPM, vía FastCGI.

**Entonces ¿qué es PHP-FPM (FastCGI Process Manager) ?**

Es una implementación de FastCGI con mejoras  si lo comparamos con el tradicional  CGI (Common Gateway Interface). Una versión mejorada de PHP que ha sido diseñada para procesar scripts de PHP de manera eficiente y rápida. En sentido general, PHP-FPM:

1. Es capaz de manipular  **múltiples solicitudes de manera simultánea** con una gestión más rápida y eficiente de las peticiones.
2. Permite **ajustar** el número de procesos PHP que se **inician**, el límite de **memoria**, el **tiempo** de espera entre otras configuraciones.
3. Permite crear **grupos de procesos** para aplicaciones diferentes en un mismo servidor, cada uno con configuraciones específicas de rendimiento y recursos, ideal para entornos de **hosting compartido**.
4. Permite **reiniciar  procesos** **sin interrumpir** el servicio, lo cual reduce los tiempos de inactividad.



## Verificar si tenemos instalado PHP-FPM

Para verificar si tenemos instalado PHP podemos hacer:

```
php -version
```

Y nos mostrará algo como lo siguiente en caso de tenerlo en el sistema

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>php -version</p></figcaption></figure>

Para comprobar si tenemos php-fpm debemos hacer:

```
php-fpm7.4 -v
```

La versión que tengo instalado es la 7.4. En caso de no saber qué versión tienes, cuando escribes en el terminal php-fpm y le das al tabulador, te mostrará la versión que tengas en el sistema.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>php-fpm7.4 -v</p></figcaption></figure>

Otro modo de verificar que todo esté OK es con systemctl:

```
sudo systemctl status php7.4-fpm
```

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Verificando php-fpm con systemctl </p></figcaption></figure>

## Instalar PHP-FPM si no lo tenemos

```
sudo apt install php-fpm
```

