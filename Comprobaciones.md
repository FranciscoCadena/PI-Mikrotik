# Comprobaciones

## Comprobacion desde Cliente de Red

Para ello tan solo montamos un equipo en virtualbox, en este caso una ubuntu desktop, y lo fundamental es que en la parte de Red, esté en red interna y el nombre corresponda con la LAN a la cual hemos montado el servicio de dhcp. 

![Red interna cliente Virtualbox](/ImagenesPI/cliente.PNG "Red interna cliente Virtualbox")

En la configuración de la red lo dejamos todo en automático, en la pestaña de _ipv4_

![Configurar ip en automatico](/ImagenesPI/redcliente.PNG "Configurar ip en automatico")

Y luego en la pestaña de _detalles_ podemos ver como la ip, el gateway y el dns corresponde a lo que nosotros definimos previamente en el servidor de DNS

![IP del cliente dada por dhcp](/ImagenesPI/ipcliente.PNG "IP del cliente dada por dhcp")

Por último comprobamos que tiene ping desde la terminal y que puede navegar por internet.

![Ping a internet y navegación](/ImagenesPI/internetcliente.PNG "Ping a internet y navegación")

## Comprobacion desde Cliente de Red por vlan

Ahora probamos a entrar en un cliente desde la vlan 10, para ello desde virtualbox tendremos nuestra ova de CHR con una interfaz interna llamada vlan 10 como se muestra a continuación.

![Ruter red vlan en virtualbox](/ImagenesPI/routerredvlan.PNG "Ruter red vlan en virtualbox")

Y un ubuntu desktop con un interfaz que pertenezca a esa misma red interna

![Cliente red vlan en virtualbox](/ImagenesPI/clienteredvlan.PNG "Cliente red vlan en virtualbox")

Una vez dentro de nuestro SO de ubuntu vamo a la configuración de red para ver si nos ha dado nuestra ip por dhcp en la vlan 10

![IP en cliente por vlan](/ImagenesPI/clienteipvlan.PNG "IP en cliente por vlan")

También podemos comprobar desde Mikrotik que ha dado ip a un equipo yendo a la pestaña de __Leases__ desde la ventana de Dhcp server, donde nos da información del equipo como la ip que se le a asignado, la MAC, nombre del host del equipo, el tiempo en el que expira la ip arrendada, etc.

![Winbox ver arrendamiento de ip por dhcp](/ImagenesPI/dhcpiparrendada.PNG "Winbox ver arrendamiento de ip por dhcp")

Probamos a hacer ping desde la terminal.

![Ping a internet](/ImagenesPI/pingvlan.PNG "Ping a internet")

Y a navegar a internet.

![Navegar por internet](/ImagenesPI/internetvlan.PNG "Navegar por internet")





