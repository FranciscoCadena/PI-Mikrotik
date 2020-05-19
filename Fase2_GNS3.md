# Fase 2 con GNS3

En este apartado se verá la configuración de todos los router usando gns3,  según la siguiente imagen, que corresponde a la Fase 2 del proyecto.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

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

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### Nat

Esta sería la regla de enmascaramiento y la regla que permite la navegación de la red lan de la sede de madrid con la red lan de la sede de sevilla por el vpn configurado.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### Routes List

La siguiente imagen corresponde a las rutas creadas para la wan y para la lan.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### VPN IPsec

A Continuación se muestra la reglas configuradas para la creación del vpn con los dos router de la sede de sevilla.
 
Lo primero son los __Peer (vecinos)__.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

En la siguiente imagen vemos las __Policies (Políticas)__ para cada vecino, donde se puede observar que uno de ellos está en _rojo_, ese corresponde al _router backup de la sede de sevilla_, con lo que si se pierde la conexión vpn con el router principal de sevilla, saltaría la conexión de vpn con el router backup de sevilla.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

Por último se ve en la siguiente imagen la conexión establecida con los vecinos activos donde se ve que se ha establecido conexión de _iniciador y respondedor_ con ambos router de sevilla.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

## Router Master Sevilla

Ahora pasamos a ver la configuración del router principal de la sede de sevilla.

### Interfaces

Lo primero es ver las interfaces que tiene, en donde se ve por el nombre dado que tiene _tres interfaces que van por WAN_, las interfaces que van a las respectivas _redes locales (DMZ, LAN2, LAN “VLAN”)_, porque interfaz van las _vlan 10 y 20_,  y los _vrrp_ creados que van a las interfaces cuyo direccionamiento de red local es estático. 

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### Dhcp y Dns

En la siguiente imagen se muestra el dns, y los dhcp server creados, los cuales van a las vlan.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

### VRRP

En la siguiente imagen se ven los vrrp creados, donde lo más importante aparte del nombre son la _Priority y el VRID_.

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

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

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")

![Fase2](ImagenesGNS3/GNSFase2/Fase2.PNG "")























