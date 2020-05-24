# Comprobaciones

# Fase 1

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

#### Ahora probaremos que la red lan1 no tendrá conexión con nadie excepto con el administrador de la lan2, la ip de uno de los trabajadores de la lan1 será 192.168.10.254.

![IP de un equipo de la lan1](/ImagenesPI/dmziplan1.PNG "IP de un equipo de la lan1")

Sin conexión a la dmz

![Sin conexión a la dmz](/ImagenesPI/dmznopinglan1dmz.PNG "Sin conexión a la dmz")

Sin conexión al administrador de servidores de la lan2

![Sin conexión al administrador de servidores de la lan2](/ImagenesPI/dmznopinglan1lan2.PNG "Sin conexión al administrador de servidores de la lan2")

Sin conexión a internet

![Sin conexión a internet](/ImagenesPI/dmznopinglan1internet.PNG "Sin conexión a internet")

Conexión con el administrador de la red lan2 que mantiene la lan1

![Conexión con el administrador de la red lan2](/ImagenesPI/dmzpinglan1lan2admin.PNG "Conexión con el administrador de la red lan2")

#### Por último haremos la configuración de la última regla NAT que añadimos para hacer la comprobación de redirección de puertos, para ello podemos usar la máquina anfitriona en este caso una windows 10,  así que se realizará la prueba desde la powershell. 

![IP del equipo anfitrión de Windows 10](/ImagenesPI/dmzipanfitrion.PNG "IP del equipo anfitrión de Windows 10")

IP configuradas del router mikrotik donde podemos observar la ip de la wan y de las diferentes LANES

![IP del router](/ImagenesPI/dmzipsrouter.PNG "IP del router")

Ip del equipo servidor de la dmz al cual vamos a entrar por ssh

![Ip del equipo servidor de la dmz al cual vamos a entrar por ssh](/ImagenesPI/dmzipserverdmz.PNG "Ip del equipo servidor de la dmz al cual vamos a entrar por ssh")

Entrando por ssh al servidor de la dmz, usando la ip de la wan del router mikrotik

![SSH de anfitrión a dmz](/ImagenesPI/dmzsshanfitriondmz.PNG "SSh de anfitrión a dmz")

Comprobación de que estamos en el servidor de la dmz viendo su ip

![Comprobación de que estamos en el servidor de la dmz viendo su ip](/ImagenesPI/dmzipdmzanfitrion.PNG "Comprobación de que estamos en el servidor de la dmz viendo su ip")

Prueba con tracepath desde el servidor de la dmz estando por ssh desde el anfitrión

![Tracepath](/ImagenesPI/dmztracepath.PNG "Tracepath")

Prueba con traceroute desde el servidor de la dmz estando por ssh desde el anfitrión

![Traceroute](/ImagenesPI/dmztraceroute.PNG "Traceroute")

## Comprobaciones de vpn por ipsec

Una de las comprobaciones que podemos hacer para ver si el túnel funciona es desde Winbox vamos al menú izquierdo, hacemos clic donde dice __Tools__ y luego a __Ping__.
En esta ventana debemos definir en la pestaña _General_  en el apartado de _Ping To_, la dirección a la cual queremos hacer ping, que en este caso será la interfaz del gateway de la red local del router con el que hemos establecido el túnel, luego vamos a la pestaña de _Advanced_ y en el apartado de _Src. Address_ escribimos la ip de la interfaz del router donde nos encontramos ahora mismo que da a la red lan de este.
Una vez configurada ambas ip, le daremos al botón de Start y en la pantalla de abajo deberá verse si hay respuesta.

![Pestaña General, ip de la lan del router destino, conexión establecida](/ImagenesPI/ipsecping.PNG "Pestaña General, ip de la lan del router destino, conexión establecida")

![Pestaña Advanced, ip de la lan del router origen, conexión establecida](/ImagenesPI/ipsecping2.PNG "Pestaña Advanced, ip de la lan del router origen, conexión establecida")

La siguiente conexión será desde un cliente de una red a otro cliente de otra red lan diferente, en este caso la ip del cliente será 192.168.20.254 como se muestra en la siguiente imagen.

![IP de cliente](/ImagenesPI/ipsecipcliente.PNG "IP de cliente")

Y primero probaremos a hacer ping a la dirección 192.168.10.1 que corresponde a la interfaz de la lan del router que está al otro lado del túnel.

![Ping a la interfaz del ruter de la otra sede](/ImagenesPI/ipsecping3.PNG "Ping a la interfaz del ruter de la otra sede")

Y seguidamente haremos lo mismo pero con la ip de un cliente que pertenezca a la red lan que está al otro lado del túnel.

![Ping a un equipo de la otra sede](/ImagenesPI/ipsecping4.PNG "Ping a un equipo de la otra sede")

# Fase 2

## Comprobar Failover

Para realizar esta prueba entraremos por Winbox a uno de los router de la empresa, tendremos en pantalla, la terminal haciendo ping al 8.8.8.8 constantemente, la herramienta traceroute haciendo ping al 8.8.8.8 y la ventana de la lista de rutas.

![Conexión con el isp principal](ImagenesPI/PIFase2/failoverprueba1.PNG "Conexión con el isp principal")

A continuación desconectamos el ISP1 que es el que definimos como principal, y observaremos como se pierde la conexión, hasta que pasado un tiempo se conectará automáticamente al ISP2 volviendo a tener conexión, no solo será visible porque vuelva a hacer ping al 8.8.8.8 desde la terminal, sino porque se verá reflejado en la ventana de Router list, cambiando de color la ruta que antes estaba en azul a negra y también cambiará la sigla definiendo que estará activa.

![Perdida de conexión con el isp principal](ImagenesPI/PIFase2/failoverprueba2.PNG "Perdida de conexión con el isp principal")

![Reconexión con el isp secundario](ImagenesPI/PIFase2/failoverprueba3.PNG "Reconexión con el isp secundario")

Por último volvemos a activar el ISP1 que antes se apagó, en cuanto se inicie deberá volver a pasar el la Route List como el principal, y en la ventana de traceroute, donde se perdió toda la conexión deberá de activarse nuevamente.

![Reconexión con el isp principal](ImagenesPI/PIFase2/failoverprueba4.PNG "Reconexión con el isp principal")


## Comprobar VRRP

Para hacer la comprobación del vrrp , entraremos en un equipo cliente con SO Ubuntu desktop, comprobaremos su ip y gateway, para confirmar que está configurado según el Vrrp asociado al Router.

![Preparación de Prueba comprobando ip y gateway del equipo cliente](ImagenesPI/PIFase2/vrrpprueba1.PNG "Preparación de Prueba comprobando ip y gateway del equipo cliente")

El siguiente paso será estar reproduciendo un video de youtube y estar haciendo ping al 8.8.8.8 continuamente desde el equipo cliente, también habrá  dos ventanas de Winbox donde veremos ambos router el maestro y el backup y dentro de cada una de estas ventanas veremos las ip e interfaces de ambos router, donde comprobaremos que el tráfico circula por el ether6-LAN del router maestro.

![Prueba de conexión](ImagenesPI/PIFase2/vrrpprueba2.PNG "Prueba de conexión")

Seguidamente para no perder la conexión con Winbox del router maestro en vez de apagarlo desconectamos el interfaz ether6-LAN que es el que está conectado al equipo cliente.

![Desconectar interfaz ether6-LAN del router maestro](ImagenesPI/PIFase2/vrrpprueba3.PNG "Desconectar interfaz ether6-LAN del router maestro")

Podemos comprobar como salta rápidamente el vrrp 1 del router backup, debido a que la respuesta es de unos 3 segundo, eso se puede confirmar viendo la ventana del equipo cliente que hace ping donde hay una parada de la secuencia del 41 al 45, apenas apreciable.
Acto seguido veremos como empieza a circular la conexión por el interfaz ether6-LAN del router backup.

![Activacion del router de respaldo por vrrp1](ImagenesPI/PIFase2/vrrpprueba4.PNG "Activacion del router de respaldo por vrrp1")

Ahora volvemos a Conectar el cable ether6-LAN del router maestro.

![Conectar interfaz ether6-LAN del router maestro](ImagenesPI/PIFase2/vrrpprueba5.PNG "Conectar interfaz ether6-LAN del router maestro")

Y en breves momento el vrrp 1 del backup vuelve a cambiar de negro a rojo, y la conexión vuelve a circular por el ether6-LAN del router maestro.

![Reconexión del vrrp1 del router maestro](ImagenesPI/PIFase2/vrrpprueba6.PNG "Reconexión del vrrp1 del router maestro")

## Comprobar Ancho de Banda

Las pruebas se realizarán desde un Ubuntu desktop que se encuentre en la red LAN2, en la cual se mostrará en una imagen la ip del equipo que deberá corresponder con la de la red asignada, y el test de velocidad.

![Prueba de velocidad antes de la regla](ImagenesPI/PIFase2/testvelocidad1.PNG "Prueba de velocidad antes de la regla.")

Seguidamente se realizará otro test de velocidad al mismo equipo con las reglas del ancho de banda ya definidas en el router y en donde deberá corresponder la ip del equipo a una de las reglas que se definen en el router.

![Prueba de velocidad después de la regla](ImagenesPI/PIFase2/testvelocidad2.PNG "Prueba de velocidad después de la regla")

Como se observa en las imágenes anteriores, al principio tenia 1Mb de subida y bajada, y después con la regla que se definió en el router tiene una subida de  0.3Mb y una bajada de  0.8Mb.

## Comprobar balanceo de carga por PCC

Para esta comprobación se a partido de una ubuntu desktop, perteneciente a la red dmz, en la imagen se muestra que la ip del equipo corresponde a dicha red, también se verá en la imagen que hay dos pestañas de internet donde se reproducen videos por youtube y desde Winbox estando conectado al router principal que le da red a la dmz, debe haber fluido de paquetes por ambas interfaces WAN.
Lo que no se puede apreciar es la suma relativa del balanceo de carga en la interfaz que da red a la dmz, esto es debido a 2 factores:
- Como se está usando una virtualización con la OVA de CHR, está restringe la conexión a 1Mb
- También como es una virtualización se requiere dos proveedores de internet dando acceso a internet, cuando en este ejemplo solo se cuenta con la red de casa.

![Comprobacion de balanceo de carga con PCC](ImagenesPI/PIFase2/pccprueba.PNG "Comprobacion de balanceo de carga con PCC")

# Fase 3

## Comprobar Port Knocking

Para realizar la comprobación del funcionamiento del port knocking primero mostraremos en una imagen las reglas creadas, para comprobar la cantidad de toques a realizar y por qué puertos debemos entrar. También se verán las interfaces e ip, en este caso nos conectaremos por la ip 192.168.0.23/24.

![Reglas firewall, interface he ip del router](ImagenesPI/PIFase3/portknockingprueba1.PNG "Reglas firewall, interface he ip del router")

Luego estaremos en la pestaña de _address list_ estando conectado al router desde Winbox, he intentaremos por ssh y por winbox al router, dándonos error.  
 
![Error de conexión al router](ImagenesPI/PIFase3/portknockingprueba2.PNG "Error de conexión al router")

En la siguiente imágenes veremos como vamos paso por paso conectándonos por ssh a los puertos definidos en las reglas, y la ip de nuestra máquina anfitriona irá apareciendo en la addresslist que corresponde a cada regla definida.

Primer toque, puerto 2000

![Llamando al puerto 2000](ImagenesPI/PIFase3/portknockingprueba3.PNG "Llamando al puerto 2000")

Segundo toque, puerto 4000

![Llamando al puerto 4000](ImagenesPI/PIFase3/portknockingprueba4.PNG "Llamando al puerto 4000")

Tercer toque, puerto 8000

![Llamando al puerto 8000](ImagenesPI/PIFase3/portknockingprueba5.PNG "Llamando al puerto 8000")

Una vez que esté en la última address list podremos entrar por ssh sin tener que definir ningún puerto. 
También se puede ver el tiempo que estará la ip de nuestra máquina anfitriona en cada address list, cuando expire el tiempo de la última address list, la que da acceso seguro para entrar al router, nos echara del router, teniendo que repetir la secuencia.

Conexión por ssh sin necesidad del puerto porque ya está en la address list que tiene permiso de conexión.

![Entrando al router sin necesidad de puertos por estar en la lista de direcciones con permisos](ImagenesPI/PIFase3/portknockingprueba6.PNG "Entrando al router sin necesidad de puertos por estar en la lista de direcciones con permisos")

Dentro de Mikrotik desde SSH, para confirmarlo se puede observar el nombre del router, las interfaces e ip tanto en el lado del winbox a la izquierda como en el lado de la conexión por ssh a la derecha de la imagen.

![Conexión establecida por ssh, y comprobación de que estamos en el router, mostrando sus ip e interfaces](ImagenesPI/PIFase3/portknockingprueba7.PNG "Conexión establecida por ssh, y comprobación de que estamos en el router, mostrando sus ip e interfaces")

__Nota__ el cambio de color del powershell es debido a que una vez dentro de Mikrotik por ssh no se veía bien las letras, de hay que haya pasado a otro color.

## Comprobar el envío de correo por gmail

Para probar que funciona correctamente el envío de correo vamos a __Tools → Email__ y en la ventana que aparece le damos a _Send Email_, nos aparecerá una nueva ventana, rellenamos los campos en donde los primeros hacen referencia a nuestra cuenta con mikrotik, y luego definimos a qué correo se lo vamos a enviar y quien se lo envía, tal como se muestra en la siguiente imagen.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

Después de darle a enviar revisamos nuestro correo para ver si nos ha llegado el mensaje y comprobar que funciona correctamente.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

## Comprobar el envío de logs asignados

Como prueba para ver si funciona el aviso de errores y dhcp creadas en el router vamos a tener abierto los logs, las reglas creadas y vamos a desconectar uno de los interfaces wan que están como dhcp cliente.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

Como vemos en el logs se a enviado un correo para notificarnos de un problema de dhcp.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

Si ahora vamos a nuestro correo podemos ver como nos han llegado varios correos avisandonos de lo ocurrido en el router tanto con el servicio de dhcp como con la interfaz, dándonos diversos detalles cada correo.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

## Comprobar la automatización de envios de Backups

Para probar de que tanto los script creados como las tareas programadas funcionan correctamente podemos modificar la hora de uno de los archivos para que correspondan con la hora que tenemos en ese momento.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

O bien desde la ventana de los Script podemos seleccionar uno de ellos y luego darle al botón de __Run Script__.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

El _Run Count_ corresponde a las veces que se a enviado por correo el script, o las veces que este se a ejecutado.
Hay que tener en cuenta que en el periodo de tiempo que definimos en las tareas es posible que de error o que no llegue algun correo si ambos _script_ deben ser enviados al mismo tiempo, por lo que recomiendo darle un margen entre un correo y otro.
 
A continuación comprobamos los correos que nos han llegado.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

Y el archivo que nos ha llegado de este

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

