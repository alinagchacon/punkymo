# Superredes IPv4

### Sumarización de redes&#x20;

En el cálculo de las subredes, hemos visto que se utilizaban direcciones de hosts para calcular más subredes. Eso permitía que fuesen redes más pequeñas, pero con mayor número de redes que parten de una red inicial.&#x20;

La sumarización de redes, también conocida como superredes o CIDR (Classless Inter-Domain Routing) realiza justamente el proceso contrario:&#x20;

* agrupa múltiples redes para que se vean como una única red. Esto se conseguirá utilizando bits de la parte de red para agrupar varias redes en una sola.&#x20;
* Para poder aplicar criterios de sumarización, es necesario que las redes que se deben agrupar sean contiguas y&#x20;
* la parte de red que deseemos utilizar en la sumarización sea la misma para todas las superredes.
