# Acceso y password

## Contraseña para acceder por la línea de consola, terminales virtuales y conexión auxiliar



### Activar contraseña para la línea de comandos

```
enable
configure terminal
hostname R1
line console 0
password cisco
login
exit
```

### Activar contraseña para acceder por  terminales virtuales

```
vty 0 5
password cisco
login
exit
```

### Activar contraseña para acceder por la conexión auxiliar

```
aux 0
password cisco
login
exit
```



Lo malo de esto es que la contraseña aparece en texto plano, sin cifrar. Lo podemos ver haciendo

```
show running-config
```

Por tanto, lo ideal es cifrar todas las contraseñas. Vamos allá:

```
service password-encryption
```

Ahora si aparecen cifradas todas las contraseñas.



## Activar contraseña en el modo EXEC privilegiado

Si queremos securizar el acceso al modo EXEC de usuario cn privilegios, podemos hacer.

```
config t
enable password cisco 
enable secret class
```

