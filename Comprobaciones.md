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

## Comprobaciones de reglas de firewall configuradas para la DMZ

#### Primero comenzaremos con el administrador de los servicios de la lan2 cuya ip es 192.168.20.2, el debe tener acceso a internet, poder hacer ping a la dmz, conectarse por ssh a la dmz, pero no debe tener acceso a la lan1.

![Lan2 salida a internet](/ImagenesPI/dmzinternet.PNG "Lan2 salida a internet")

Ping a uno de los servidores de la dmz

![Ping de lan a dmz](/ImagenesPI/dmzping.PNG "Ping de lan a dmz")

Conexión por ssh a uno de los servidores de la dmz

![Conexion por ssh a la dmz](/ImagenesPI/dmzssh.PNG "Conexion por ssh a la dmz")

No conexión con un trabajador de la lan1

![No hay ping a la lan1](/ImagenesPI/dmznopinglan1.PNG "No hay ping a la lan1")

#### En cambio el otro administrador de la lan2 cuya ip es 192.168.20.3 y que se encarga de la red lan1 si tendrá conexión a esta y a internet pero no a la dmz ni tampoco por ssh.

IP del administrador de la lan2 que se encarga de mantener la lan1

![IP del administrador de la lan2 que se encarga de mantener la lan1](/ImagenesPI/dmziplan2.PNG "IP del administrador de la lan2 que se encarga de mantener la lan1")

Sin conexión a la dmz

![Sin conexión a la dmz](/ImagenesPI/dmznopinglan2dmz.PNG "Sin conexión a la dmz")

Conexión a un trabajador de la red lan1

![Conexión a la red lan1](/ImagenesPI/dmzpinglan2lan1.PNG "Conexión a la red lan1")

#### Pruebas con un servidor de la dmz cuya ip será 192.168.30.3, que deberá tener acceso a internet, conexión con el administrador de servidor de la lan2, y todo lo demás denegado.

![IP de servidor de dmz](/ImagenesPI/dmzipserver.PNG "IP de servidor de dmz")

Ping con el Administrador de la red lan2

![Ping con el Administrador de la red lan2](/ImagenesPI/dmzpingdmzlan2.PNG "Ping con el Administrador de la red lan2")

No conexión con la red lan1

![No conexión con la red lan1](/ImagenesPI/dmznopingdmzlan1.PNG "No conexión con la red lan1")

#### Por último la red lan1 no tendrá conexión con nadie excepto con el administrador de la lan2, la ip de uno de los trabajadores de la lan1 será 192.168.10.254.

![IP de un equipo de la lan1](/ImagenesPI/dmziplan1.PNG "IP de un equipo de la lan1")

Sin conexión a la dmz

![Sin conexión a la dmz](/ImagenesPI/dmznopinglan1dmz.PNG "Sin conexión a la dmz")

Sin conexión al administrador de servidores de la lan2

![Sin conexión al administrador de servidores de la lan2](/ImagenesPI/dmznopinglan1lan2.PNG "Sin conexión al administrador de servidores de la lan2")

Sin conexión a internet

![Sin conexión a internet](/ImagenesPI/dmznopinglan1internet.PNG "Sin conexión a internet")

Conexión con el administrador de la red lan2 que mantiene la lan1

![Conexión con el administrador de la red lan2](/ImagenesPI/dmzpinglan1lan2admin.PNG "Conexión con el administrador de la red lan2")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")







