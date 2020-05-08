# La planificación se realizara en diferentes fases
## La primera fase sera montar una topografia de red como en la siguiente imagen.
![Fase1 de Red](/ImagenesPI/FASE1.PNG)

__En la cual se configurara los siguientes aspectos__
- DHCP tanto cliente como servidor
- DNS
- Dar ip a las interfaces y nombrarlas para diferenciarlas.
- Crear redes internas.
- Crear una DMZ y uso de reglas de firewall.
- Creaciones de vlan y bridges.
- Creación de vpn entre dos router de diferentes redes con IPsec.
- Crear reglas básicas de NAT en el firewall, como enmascaramiento.

Todo este apartado estara en el documento de configuracion de una red empresarial.

## La segunda fase tendrá algunos cambios en la topografía de red respecto a la anterior como se ve en la siguiente imagen.
![Fase1 de Red](/ImagenesPI/FASE1.PNG)

__En esta parte implementaremos algunos tecnologias para que nuestra red tanga mayor disponibilidad__
- Se implementará un Failover de líneas de respaldo para ambos router con los dos ISP, para cuando uno de ellos falle, automáticamente tire por el otro.
- Se implementará un VRRP entre los dos router de la empresa para las redes estáticas de la DMZ y la LAN2 con ello ayudamos a que cuando un router se dañe sigamos teniendo conexión gracias a que tirara por el otro router, es un proceso muy parecido al failover.





