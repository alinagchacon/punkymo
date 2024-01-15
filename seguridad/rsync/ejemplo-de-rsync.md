---
description: Un ejemplo completo
---

# Ejemplo de rsync

Supongamos que tenemos que hacer copias de seguridad en un servidor de backups, utilizando el comando rsync. SUpongamos que se quiere mantener actualizado un directorio de un equipo cliente. Para ello debemos:

* verificar que existe el directorio del equipo cliente&#x20;
* verificar los archivos contenidos en el directorio
* crear un script que realice todo esto y además ejecute el backup
* configurar el programador de tareas: crontab

¿Cómo podemos probarlo?

Os propongo dos opciones:

* **Equipo host y una VM**.&#x20;
  * El `host` hace de server y la `VM` es el equipo cliente. Desde el host ejecutamos el script y donde se almacenan las copias. Un detalle es que tengo instalado Ubuntu en mi PC.&#x20;
  * El `host` hace de cliente (Windows) y la `VM` es el server. Desde la VM ejecutamos el script y donde se almacenan las copias.
* **Dos VM**.
  * Una de las VM hace de server y la otra de cliente. Ambas conectadas a una red NAT.





```
#!/bin/bash

   scr="source/"
   dst="/home/kirby/Documentos/1.iFP/5.MIS_APUNTES/servicios-de-red/scripting/funciona-ok-en-1PC/destino/"
   cliente=$(cat clienteRemoto.txt)

   testRemoteDir(){
        for host in $cliente; do

           sshpass -p $(cat sshpass.txt)  ssh $host 'bash -s' < detest.sh "$scr $dst $host"
           echo "Desde test.sh digo que: Return: $?"

           if [ -n "$?" ]
             then
                echo "Hago la copia"
                 sshpass -p $(cat sshpass.txt) rsync -avzh -e ssh $(cat clienteRemoto.txt):~/$scr/  $dst

            else
                echo "No hice nada: $res"
           fi
        done

   }


  testRemoteDir
  echo "llegué al final"

```



### Links

* [https://geekland.eu/ejecutar-comandos-y-scripts-con-ssh-en-equipos-remotos/](https://geekland.eu/ejecutar-comandos-y-scripts-con-ssh-en-equipos-remotos/)
* [https://itsfoss.com/es/funciones-bash/](https://itsfoss.com/es/funciones-bash/)
* [https://atareao.es/tutorial/scripts-en-bash/funciones-en-bash/](https://atareao.es/tutorial/scripts-en-bash/funciones-en-bash/)



