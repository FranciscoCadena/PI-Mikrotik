# Fase 2 con GNS3

En este apartado se verá la configuración de todos los router usando gns3,  según la siguiente imagen, que corresponde a la Fase 2 del proyecto.

![Topografia de la Fase 2 Virtualizado con GNS3](ImagenesGNS3/GNSFase2/gns3fase2.PNG "Topografia de la Fase 2 Virtualizado con GNS3")

Como se puede observar en la imagen anterior, la topografía de red ha sido simplificada con respecto a lo visto del diagrama de la fase dos del apartado [Planificación de la Red Empresarial](https://github.com/FranciscoCadena/PI-Mikrotik/blob/master/Planificaci%C3%B3n_de_la_Red_de_una_empresa.md).

Esto es debido porque a la hora de simular dos proveedores de internet, no es posible hacerlo usando solo la red que te da tu propio router de casa, por ello como sustitución de los dos ISP se a usado todos los interfaces que se pueden incorporar a la _OVA de GNS3_ que está siendo virtualizado por _Virtualbox_.

Esto quiere decir que todas las _nubes_ que se ven en el diagrama corresponden a la _ova de gns3_, el cual consta de __tres interfaces__, uno en modo anfitrión, otro en modo Nat y el último en modo Bridge, usando estos dos últimos que tienen salida a internet como si fuesen dos ISP.

También se a _simplificado las vlan_, porque sabiendo configurar dos, se sabe configurar todas las que se quieran, con ello es más fácil de entender y simplificar las reglas que tendrán cada router.

Cabe también destacar que el Router que está en la red de las vlan, es un Switch siendo simulado con el sistema operativo RouterOS, de hay que la imagen sea como la de un router.
 
A continuación se mostrará la configuración de cada uno de los router, para esta topografía de red donde ya tienen incorporado las herramientas de Alta disponibilidad que se explicaron en el documento de [Configuración de Herramientas de alta Disponibilidad](https://github.com/FranciscoCadena/PI-Mikrotik/blob/master/Configuraci%C3%B3n_Herramientas_Alta_Disponibilida.md).
De hay que solo se muestre la configuración final, sin entrar en detalles de como configurarlos.

## Router Sede Madrid

### Quick Set

Se puede observar, que el router de la sede de Madrid tan solo trabaja con dos interfaces una WAN con ip estática, y una red LAN.

![Configuración Rapida del Router de Madrid](ImagenesGNS3/GNSFase2/gns3madridquickset.PNG "Configuración Rapida del Router de Madrid")

### Nat

Esta sería la regla de enmascaramiento y la regla que permite la navegación de la red lan de la sede de madrid con la red lan de la sede de sevilla por el vpn configurado.

![Reglas NAT del Router de Madrid](ImagenesGNS3/GNSFase2/gns3madridnat.PNG "Reglas NAT del Router de Madrid")

### Routes List

La siguiente imagen corresponde a las rutas creadas para la wan y para la lan.

![Lista de Rutas del Router de Madrid](ImagenesGNS3/GNSFase2/gns3madridroutes.PNG "Lista de Rutas del Router de Madrid")

### VPN IPsec

A Continuación se muestra la reglas configuradas para la creación del vpn con los dos router de la sede de sevilla.
 
Lo primero son los __Peer (vecinos)__.

![Pestaña Peer del Router de Madrid](ImagenesGNS3/GNSFase2/gns3madridpeers.PNG "Pestaña Peer del Router de Madrid")

En la siguiente imagen vemos las __Policies (Políticas)__ para cada vecino, donde se puede observar que uno de ellos está en _rojo_, ese corresponde al _router backup de la sede de sevilla_, con lo que si se pierde la conexión vpn con el router principal de sevilla, saltaría la conexión de vpn con el router backup de sevilla.

![Pestaña Policies del Router de Madrid](ImagenesGNS3/GNSFase2/gns3madridpolicies.PNG "Pestaña Policies del Router de Madrid")

Por último se ve en la siguiente imagen la conexión establecida con los vecinos activos donde se ve que se ha establecido conexión de _iniciador y respondedor_ con ambos router de sevilla.

![Pestaña Active Peer del Router de Madrid](ImagenesGNS3/GNSFase2/gns3madridactivepeer.PNG "Pestaña Active Peer del Router de Madrid")

## Router Master Sevilla

Ahora pasamos a ver la configuración del router principal de la sede de sevilla.

### Interfaces

Lo primero es ver las interfaces que tiene, en donde se ve por el nombre dado que tiene _tres interfaces que van por WAN_, las interfaces que van a las respectivas _redes locales (DMZ, LAN2, LAN “VLAN”)_, porque interfaz van las _vlan 10 y 20_,  y los _vrrp_ creados que van a las interfaces cuyo direccionamiento de red local es estático. 

![Interfaces del Router Master de Sevilla](ImagenesGNS3/GNSFase2/gns3masterinterface.PNG "Interfaces del Router Master de Sevilla")

### Dhcp y Dns

En la siguiente imagen se muestra el dns, y los dhcp server creados, los cuales van a las vlan.

![Dhcp y DNS del Router Master de Sevilla](ImagenesGNS3/GNSFase2/gns3masterdhcpdns.PNG "Dhcp y DNS del Router Master de Sevilla")

### VRRP

En la siguiente imagen se ven los vrrp creados, donde lo más importante aparte del nombre son la _Priority y el VRID_.

![VRRP creados en el Router Master de Sevill](ImagenesGNS3/GNSFase2/gns3mastervrrp.PNG "VRRP creados en el Router Master de Sevilla")

### IP de las interfaces

En la siguiente imagen se ve el direccionamiento de cada interfaz, vlan y vrrp.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### VPN por IPsec

Esta sería la configuración de Peer con la sede de Madrid.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

Seguidamente vemos la Política asociada.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

Y por último el vecino Activo.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### Nat

En la siguiente imagen vemos las reglas de Nat creadas donde en primer lugar está la regla que permite la conexión de las redes locales entre las sedes de Sevilla y Madrid que van por vpn.
Luego el enmascaramiento de las tres interfaces wan.
Y por último las redirecciones de puerto para los servidores.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### Firewall

Estas serían las reglas de firewall, donde no hay muchos cambios con las ya creadas en la Fase 1.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### Mangle

En la siguiente imagen se ve las reglas creadas para el balanceo de carga por PCC, donde aunque tengamos tres interfaces WAN, solo se han usado dos de estas para el balanceo, el cual va dirigido para la DMZ.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### Routes List

En la siguiente imagen vemos todas las rutas creadas, en donde se puede ver el __failover__ definido por la _distance_, estos están en color celeste y en modo estático, esperando a que el principal caiga y activarse la siguiente que esté definida.
También se puede ver los dos interfaces que se están usando para el __balanceo de carga__ donde dice _routing mask_ que hacen referencia a que proveedor de internet va cada uno.
Podemos ver también las rutas correspondientes para las _vlan y los vrrp_.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

En esta imagen podemos ver la configuración del _ancho de banda_ el cual consta de dos reglas una definida para toda la red LAN2 y otra que va primero para definir un determinado equipo dentro de la red LAN2.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")























