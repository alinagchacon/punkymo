# MySQLi - PHP

Verificar que tenemos mysqli instalado:

```
php -m | grep mysqli
```

En caso de no tenerlo, instalar:

```
sudo apt install php-mysqli
```

Verificar que tengamos habilitado el módulo de mysqli en php.ini tanto para php como php-fpm. Para asegurarnos de cual archivo `php.ini` se esté cargando (teniendo en cuenta que también tenemos el módulo de php-fpm) podemos hacer:

```
php -i | grep "php.ini"
```

Y se nos mostrará:

<figure><img src="../../../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>Archivo de php.ini </p></figcaption></figure>

Una vez que tengamos localizado el archivo php.ini tenemos que buscar la línea “_extension=mysqli_” or “_extension=php\_mysqli_“ y borramos el ";" para descomentar la línea.

Verificar que tengamos habilitado el módulo de mysqli en php.ini tanto para php como php-fpm:

```
/etc/php/{PHP_VERSION}/cli/php.ini

more /etc/php/7.4/cli/php.ini
more /etc/php/7.4/fpm/php.ini
```

Deberíamos ver algo como:

<figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption><p>Módulo mysqli en php.ini</p></figcaption></figure>

Importante restablecer los servicios:

```
sudo systemctl restart nginx
```



