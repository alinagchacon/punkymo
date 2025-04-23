# SRT

Los servidores SRT se basan en el protocolo SRT que es de código abierto.   Dicho protocolo permite transmisiones de video, en vivo o utilizando vídeos grabados, en tiempo real por internet. &#x20;

Para la transmisión, el protocolo SRT utiliza UDP en lugar de TCP. Sin embargo, SRT añade características interesantes a UDP que hacen que el protocolo sea seguro y confiable.

A fin de garantizar la seguridad de la transmisión, SRT  usa el cifrado AES-128 o AES-256. Por tanto, los receptores deben conocer la clave para el procesamiento de la señal.&#x20;

Dado que Internet no es una red estable es frecuente que haya pérdida de datos durante las transmisiones.  Para evitar la pérdida de información SRT utiliza el protocolo ARQ, que comprueba si todos los bloques de datos han llegado al receptor y los devuelve a la unidad transmisora. En caso de que no se reciba la confirmación de un paquete, se reenviaría nuevamente. El proceso resultante es muy rápido y las interrupciones son imperceptibles durante la transmisión.





## Links

* [https://github.com/Haivision/srt](https://github.com/Haivision/srt)
*

