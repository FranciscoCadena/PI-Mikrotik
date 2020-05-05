# Fase 1 con GNS3
En este apartado veremos el esquema de la topografía de red con gns3 según la fase 1, y como queda la configuración del router principal.

![topografía de red con gns3](/imagenesGNS3/fase1gns3.PNG "topografía de red con gns3")

Direcciones ip de las diferentes interfaces y bridges configuradas.

![Direccionamiento](/imagenesGNS3/direcciones.PNG "Direccionamiento")

Interfaces, vlans y bridges con nombres a las que estan configuradas.

![Interfaces definidas](/imagenesGNS3/interfaces.PNG "Interfaces definidas")

Dhcp cliente para la WAN que recibe la ip de un ISP que en este caso es el router de casa.

![dhcp cliente](/imagenesGNS3/dhcpcliente.PNG "dhcp cliente")

Dhcp server para los bridges.

![Dhcp server bridges](/imagenesGNS3/dhcpserver.PNG "Dhcp server bridges")

Dhcp server con las redes, gateway y dns.

![Dhcp server Network](/imagenesGNS3/dhcpserverredes.PNG "Dhcp server Network")

Dhcp server con las ip arrendadas.

![Dhcp server Leases](/imagenesGNS3/dhcpserverarrendada.PNG "Dhcp server Leases")

DNS.

![DNS](/imagenesGNS3/dns.PNG "DNS")

Lista de rutas.

![Route List](/imagenesGNS3/lista_de_rutas.PNG "Route List")

Configuracion de los puertos de los bridges donde se observa con que vlan e interfaces estan puenteadas.

![Bridges Ports](/imagenesGNS3/bridgesports.PNG "Bridges Ports")

Configuración de Nat por Firewall.

![Firewall NAT](/imagenesGNS3/firewallnat.PNG "Firewall NAT")

Reglas de Firewall.

![Firewall Rules](/imagenesGNS3/firewallreglas.PNG "Firewall Rules")

Politicas del tunel por IPsec.

![IPsec Policies](/imagenesGNS3/IPsecpolitica.PNG "IPsec Policies")

Vecinos que estan activos por el tunel creado.

![Ipsec Active Peers](/imagenesGNS3/ipsecvecinoactivo.PNG "Ipsec Active Peers")








