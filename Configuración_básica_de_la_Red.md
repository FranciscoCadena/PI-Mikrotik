# Datos del documento

En este documento veremos lo siguiente:
- Tipos de licencia de Mikrotik 
- Donde descargar el CHR de Mikrotik y la herramienta de Winbox
- Conectarnos a mikrotik por Winbox
- Cambiar nombre de usuario y pass al acceder a mikrotik
- Resetear nuestro router
- Configuración de Dhcp cliente y servidor
- Donde se configura el Dns y el gateway
- Nombrar y crear ips
- Crear vlans y hacerlo con un solo router
- Crear un Vpn con protocolo IPsec
- Configurar reglas de Firewall para una DMZ

## Licencias Mikrotik, RouterOS y Winbox

Nuestro primer paso será ir a la página oficial de [Mikrotik](https://mikrotik.com/download), una vez dentro vamos a la pestaña de Software y luego a Download, aquí hay que tener algo en cuenta, el sistema operativo __RouterOS__ va por licencias, aquí podemos ver los distintos tipos de licencia y lo que nos ofrece cada una sacada directamente de la página oficial de mikrotik.

![Tabla de las distintas licencias de Mikrotik](/ImagenesPI/License.PNG "Tabla de las distintas licencias de Mikrotik")

Como vemos hay una licencia gratuita en plan de demostración, pero como es de prueba no trae todas las características, hay que tener en cuenta que estas licencias solo es necesario comprarlas si queremos su software para tener un PC como enrutador, ya que los diferentes productos de Hardware de mikrotik al comprarlos ya te vienen con un nivel de licencia el cual te lo especifican en sus características. Esto es algo importante porque en varios videotutoriales no te lo especifican, usando licencias de prueba o algunos que ya tienen comprado sin especificarlo.

Por lo tanto nosotros debemos descargar el __Cloud Host Router__, que no es más que una versión gratuita con todas las herramientas del RouterOS con la única pega de que la velocidad de Navegación será de 1 MB, esta versión te viene en varios formatos para poder ser virtualizado, por ello descargamos la versión OVA template para que sea reconocida por Virtualbox y de ellas la versión Stable.

![Imagen con las diferentes imágenes de CHR y sus versiones para descargar](/ImagenesPI/CHR.PNG "Imagen con las diferentes imágenes de CHR y sus versiones para descargar")

Una vez descargada el procesos para instalarlo en Virtualbox es el mismo que el de cualquier OVA importada, en cuanto la ova este importada tan solo deberemos definir las interfaces de red que vamos a usar que en este caso usaremos las 4, y definimos de qué tipo serán con lo que la ether 1 será la WAN así que la dejaremos en modo bridge para que coja la ip por DHCP de nuestro router de casa, y las otras 3 interfaces las pondremos como redes internas para la DMZ y las 2 redes LAN.

En cuanto lo tengamos listo la encendemos. Y lo primero que nos pedirá será el login, el cual por defecto será __admin__ y de password ninguno.

![Login Terminal RouterOS](/ImagenesPI/Login.PNG "Login Terminal RouterOS")

En cuanto entremos podemos observar que es una terminal que trabaja por línea de comando, debido a que su infraestructura deriva de Linux, en la siguiente imagen con el comando __ip address print__ podemos ver que la interfaz que se definió como puente en virtualbox ya tiene una ip asignada.

![Terminal ip address print](/ImagenesPI/adresprint.PNG "Terminal ip address print")

Y si usamos el comando __interface print__ nos dara informacion de todas las interfaces conectadas.

![Terminal interface print](/ImagenesPI/interfaceprint.PNG "Terminal interface print")

El hecho de que el Software de mikrotik trabaje por línea de comandos tiene sus partes positivas y negativas.

Como parte negativas, nos encontramos que debemos aprender nuevos comandos, y también según algunos administradores de redes de foros que algunas configuraciones avanzadas deben hacerse vía CLI, de ahí que mikrotik al igual que Cisco tengan sus propios cursos y certificados para aprender todo lo que nos puede ofrecer. 

Como parte positiva, podemos crear script para automatizar tareas o incluso para cargar configuraciones de red si estas son las mismas o similares, desde github muchas personas ponen a disposición de todos, script para realizar múltiples tareas y diferentes configuraciones.

Pero Mikrotik también puede ser configurado de forma gráfica, vía web o con herramientas como __Winbox__ la cual será la que usaremos.

Para ello podemos descargar Winbox desde la propia página de [Mikrotik](https://mikrotik.com/download), tan solo debemos elegir la versión de 32 o la de 64 bit, una vez descarga su instalación es muy simple tan solo hay que elegir la ruta donde se instalará y darle a siguiente.

![Descargar WinBox](/ImagenesPI/winboxdownload.PNG "Descargar WinBox")

En cuanto abrimos Winbox nos aparecerá una pantalla como la siguiente en la cual el mismo empezará a rastrear los diferentes equipos de Mikrotik que haya. 

![Entrar por MAC](/ImagenesPI/WBlogin.PNG "Entrar por MAC")

Como observamos en las imágenes ha detectado 2 equipos de Mikrotik, que en este caso ambos son CHR, pero uno tiene IP y el otro no, esto se debe porque al bajar la OVA de CHR ya está configurado para que de IP pero la otra OVA como a sido reseteada pierde esa configuración que trae por defecto, pero Winbox puede entrar directamente por la MAC, y una vez dentro ya configurar la ip de las Interfaces, y luego ya entrar por la ip, nombre de admin y password que hayamos definido en su primera configuración.

![Entrar por IP](/ImagenesPI/WBlogin2.PNG "Entrar por IP")

Una vez seleccionado un equipo le damos a __Connect__ y nos aparecerá la pantalla principal de Winbox que es la siguiente.

![Winbox Interface](/ImagenesPI/WBindex.PNG "Winbox Interface")

Prácticamente en el menú izquierdo tendremos todas las herramientas necesarias para hacer las configuraciones necesarias a nuestro Router. 

## Configuración 

### Reset

Como primeros pasos empezaremos por resetear la configuración del router, para ello desde el menu izquierdo vamos a __System → Reset__ Configuration
En la nueva ventana marcamos las casillas No Default Configuration y Do Not Backup, y luego le damos al botón de __Reset Configuration__

![Reseteo de Routeros](/ImagenesPI/Reset.PNG "Reseteo de Routeros")

### Login y Password

Nuestro siguiente paso será darle un nombre de usuario y una contraseña, para ello vamos a __System → Users__ y nos aparecerá el usuario admin que viene por defecto, vamos a  agregar un nuevo usuario dándole al símbolo del más, le damos un nombre, contraseña y lo metemos en el grupo de full para que tengo los privilegios de un admin. Le damos al botón de Apply y luego a OK, una vez creado marcamos al usuario admin y lo podemos eliminar o deshabilitar, pulsando el botón menos o la cruz respectivamente.

![Usuario y Pass](/ImagenesPI/Userpass.PNG "Usuario y Pass")

Ahora si nos desconectamos y nos volvemos a conectar veremos que nos impedirá entrar con el user admin, teniendo que usar nuestro nuevo usuario creado. Para desconectarnos tan solo le damos al botón __Session__ de la barra de herramientas y luego a __Disconnect__.

![Error al conectarse](/ImagenesPI/erropass.PNG "Error al conectarse")

### Nombrar Interfaces

El siguiente paso que vamos a hacer es definir las interfaces dando a cada una un nombre para poder diferenciarlas y que correspondan con lo definido en las interfaces de virtualbox.
Para ello tan solo debemos hacer doble clic en cada interfaz y darle un nombre en la ventana que nos aparecerá y luego a OK.
Si por el motivo que sea no tenemos la ventana de las interfaces que sale por defecto al iniciar Winbox, tan solo debemos hacer clic a donde dice __Interfaces__ en el menú izquierdo.

![Nombrear interfaz de WAN](/ImagenesPI/wan.PNG "Nombrear interfaz de WAN")

![Interfacez nombradas](/ImagenesPI/interfaces.PNG "Interfacez nombradas")

Hay que tener en cuenta que todo lo que estamos haciendo en Winbox directamente lo podemos ver reflejado en la terminal del RouterOS por ello si abrimos una terminal desde Winbox dándole a __New terminal__ desde el menú izquierdo y ejecutamos el comando antes usado de __interface print__ veremos como ahora todas las interfaces están nombradas.

![Terminal en Winbox](/ImagenesPI/WBterminal.PNG "Terminal en Winbox")

### WAN por DHCP

El siguiente paso será darle ip a la WAN, como está configurado como modo puente en virtualbox, haremos que reciba la ip en modo cliente DHCP y que sea el Router de casa el que se lo facilite.
Para ello en el menu izquierdo vamos a __IP → DHCP Cliente__, en la nueva ventana le damos al símbolo del más, y en la nueva ventana seleccionamos la interface que en este caso será la WAN y le damos al botón de OK.

![Configurando DHCP cliente en la WAN](/ImagenesPI/dhcpcliente.PNG "Configurando DHCP cliente en la WAN")

![WAN configurada y con ip dada por dhcp](/ImagenesPI/wandhcp.PNG "WAN configurada y con ip dada por dhcp")

### IP estaticas

Hemos visto que la WAN la hemos dejado como cliente de dhcp, pero si fuese un ISP con un ip publica estatica tambien se puede hacer, mostraremos cómo se realiza y de paso configuraremos ip estáticas para las redes LAN y la DMZ.
Para ello vamos al menú izquierdo, le damos a __IP → Addresses__, nos aparecerá una ventana donde estará solo la ip de wan dada por dhcp, así que nosotros le damos al símbolo del mas para agregar una ip.

![IP dinamica de la WAN](/ImagenesPI/ipwan.PNG "IP dinamica de la WAN")

Al darle al símbolo del mas, en la nueva ventana que nos aparece debemos definir la interfaz a la cual le vamos a dar la ip, escribimos su ip en addresses con su máscara de red y al darle a aplicar ya rellenara Winbox el apartado de Network definiendo la red a la que pertenece. Esto lo haremos con todas las interfaces de las LAN y la DMZ, si nuestro Router es un ISP y tiene una ip pública la definimos aquí, como no es el caso podemos dejarlo como cliente de dhcp. 

![Configurar ip estatica](/ImagenesPI/ipestatica.PNG "Configurar ip estatica")

Una vez todas las interfaces configuradas con su ip nos quedaría de esta manera.

![IPs configuradas](/ImagenesPI/ipslanes.PNG "IPs configuradas")

### Configurar enmascaramiento

Para ello desde el menú izquierdo debemos ir a __IP → Firewall__, una vez aquí vamos a la pestaña de __NAT__, y le damos al botón de más.

![Firewall NAT](/ImagenesPI/NAT.PNG "Firewall NAT")

En la nueva ventana estando en la pestaña de __general__, debemos asegurarnos que donde dice _chain_ este __srcnat__ que es lo mismo que source nat, y en out interface debe ser la __WAN__.  

![srcnat](/ImagenesPI/srcnat.PNG "srcnat")

Seguidamente vamos a la pestaña de Action, y donde dice Action debemos seleccionar masquerade.

![masquerade](/ImagenesPI/masquerade.PNG "masquerade")

Al final le damos a Apply y luego a OK

![enmascaramiento configurado](/ImagenesPI/natconfi.PNG "enmascaramiento configurado")

### Gateway

Para configurar la ruta de enlace por defecto, vamos al menú izquierdo, y le damos a __IP → Route List__, al tener la WAN por dhcp Winbox ya lo tendrá configurado por defecto, pero si tenemos que hacerlo de forma manual, tan solo le damos al símbolo del mas, y en la nueva ventana definimos el gateway que en este caso es el Router de casa, y las direcciones ip las dejamos en 0.0.0.0/0 para definir que son todas, aplicamos y le damos a ok.

![Gateway](/ImagenesPI/gateway.PNG "Gateway")

### DNS

Para configurar los DNS tan solo debemos ir desde el menú izquierdo a __IP → DNS__, donde en server podemos añadir los dns que queramos, en este ejemplo se están usando las dns públicas de google, para añadir más tan solo le damos a la flecha de abajo que hay justamente a la derecha de la pantalla del server, con ello se abrira otra pestaña para agregar otra dirección dns.
Si marcamos la opción de Allow Remote Requests, eso hará que nuestro dispositivo Mikrotik pase a ser un servidor dns para toda nuestra red LAN. Aplicamos y le damos a OK.

![DNS](/ImagenesPI/dns.PNG "DNS")

Ahora podemos probar si nuestro equipo Mikrotik tiene salida a internet, desde Winbox podemos abrir una terminarl y hacer ping a [Google](www.google.es) por ejemplo como se muestra en la imagen siguiente.

![Ping desde terminal de winbox](/ImagenesPI/ping1.PNG "Ping desde terminal de winbox")

### DHCP server

Ahora vamos a montar un servicio dhcp para los clientes de la LAN, esto es opcional ya que como anteriormente configuramos nuestra ip estática a cada interfaz correspondiente a cada LAN podríamos ir a cada equipo cliente de la LAN y establecer una ip estática definiendo su gateway que corresponda con los definidos en nuestro equipo Mikrotik.
Para crear nuestro servicio dhcp vamos desde el menú izquierdo a __IP → DHCP server__.

![Dhcp server](/ImagenesPI/dhcp.PNG "Dhcp server")

En esta ventana podemos darle al botón del mas y se nos abrirá una ventana donde configuramos los parámetros de nuestro servidor dhcp, como el nombre, que interfaz será la que tendrá el servicio de dhcp, el tiempo de concesión de la ip, el rango de ip, etc.

![Configurar Dhcp server](/ImagenesPI/dhcpserver.PNG "Configurar Dhcp server")

Pero también tenemos una forma de hacerla de forma guiada para ello le damos en vez de al boton mas al boton que dice DHCP Setup.

![Dhcp setup LAN](/ImagenesPI/dhcpsetup.PNG "Dhcp setup LAN")

Como vemos en la imagen lo primero que nos pedirá es que interfaz corresponderá al servicio de dhcp, en este ejemplo usamos la LAN1 y luego le damos a Next.

![Dhcp setup Red](/ImagenesPI/dhcpsetup2.PNG "Dhcp setup Red")

A continuación nos saldrá la dirección de red que corresponderá con el servidor de dhcp, dejamos la que viene por defecto y le damos a Next.

![Dhcp setup gateway](/ImagenesPI/dhcpsetup3.PNG "Dhcp setup gateway")

La siguiente pantalla hace referencia a cuál será el gateway de dicha red, comprobamos que sea correcta, con la ip que definimos para la red lan 1 y le damos a next.

![Dhcp setup rango](/ImagenesPI/dhcpsetup4.PNG "Dhcp setup rango")

La siguiente ventana es para definir el rango de ip que dará el servicio dhcp, como ejemplo podemos definir que de ip desde .100 hasta la .254 dejando un rango de ip libres por si necesitamos montar algún equipo con una ip fija.

![Dhcp setup dns](/ImagenesPI/dhcpsetup5.PNG "Dhcp setup dns")

La siguiente ventana hace referencia a los servidores de DNS que usará, en los cuales los 2 primeros hacen referencia a los que nos proporciona el Router de casa, y los 2 siguientes a los que nosotros añadimos cuando configuramos el DNS.

![Dhcp setup arrendamiento](/ImagenesPI/dhcpsetup6.PNG "Dhcp setup arrendamiento")

Por último nos preguntará el tiempo de arrendamiento de la ip de nuestro cliente.

![Dhcp setup configurado](/ImagenesPI/dhcpsetup7.PNG "Dhcp setup configurado")

Con el último paso ya habremos terminado de montar nuestro servicio de dhcp ahora solo queda montar un equipo de la LAN1 y ver si recibe una ip del rango definido y que tenga salida a internet.

## Vlan

Nuestro siguiente paso será crear redes virtuales para una LAN, por ejemplo supongamos que en una empresa en una misma LAN hay varios sectores por ejemplo: un grupo de Recursos Humanos, otro grupo de Administrativos, otro de Secretarios. Y para cada uno de estos grupos nombrados queremos que aunque estén en la misma red tengan sus propias redes diferenciadas una de las otras, pues para ello crearemos las Vlan, RouterOS admite __hasta 4095 interfaces vlan__, cada una con una ID de vlan única, por interfaz.

Para guiarnos nos fijamos en el siguiente diagrama en donde habrá 3 equipos en una misma LAN, pero cada uno tendrá su propia vlan diferenciada, por lo tanto configuraremos el Router con las 3 vlan en la interfaz de la LAN1, y luego para simular el switch lo haremos otro RouterOS donde designaremos cada vlan a un interfaz en modo bridge.

![Diagrama de red simple con 3 Vlan](/ImagenesPI/redvlanes.PNG "Diagrama de red simple con 3 Vlan")

Primero vamos al Router que terminamos de configurar al principio, nos vamos a interfaces desde el menu izquierdo, vamos a la _pestaña de Vlan_ y  le damos al símbolo más.

![Crear Vlan](/ImagenesPI/vlna1.PNG "Crear Vlan")

En esta nueva ventana definimos el nombre de la vlan, el tipo que será una vlan, su identificador, y a que interfaz irá vinculada. Terminado le damos a Apply y luego a OK.

![Crear Vlan30](/ImagenesPI/vlan30.PNG "Crear Vlan30")

Una vez configurada las 3 vlan, debemos darle sus ip, para ello desde el menu izquierdo vamos a __IP → Addresses__. le damos al símbolo del mas, y seleccionamos cada una de las vlan creadas y le damos una ip por ejemplo: 
- Vlan10 → 10.10.10.1
- Vlan20 → 20.20.20.1
- Vlan30 → 30.30.30.1

![Direccionamiento de las Vlan](/ImagenesPI/ipvlan.PNG "Direccionamiento de las Vlan")

Terminada la configuración  del router, lo que faltaria hacer seria conectar la interfaz del Router que corresponde a la LAN1 al interfaz principal del Switch. Hecho esto entramos en el Switch por el RouterOS para configurarlo.

El primer paso será parecido al realizado en el Router, vamos a seleccionar la interface ether1 que será la conectada al router y crearemos las 3 vlan que creamos en el Router. Como en el paso anterior le damos al símbolo del más estando en la ventana de las interfaces, y después a Vlan, y rellenamos los campos como hicimos en el Router, hasta tener las 3 vlan creadas.

![Crear vlan 10](/ImagenesPI/crearvlan10.PNG "Crear vlan 10")

![Creadas las 3 vlanes](/ImagenesPI/vlans.PNG "Creadas las 3 vlanes")

Una vez creada las vlan vamos a vincularla cada una con un interfaz del switch para eso haremos un bridge, con lo que nuestro siguiente paso será ir al menú izquierdo y luego a Bridge, y seguidamente al símbolo más para crearlo.

![Crear bridge](/ImagenesPI/bridge.PNG "Crear bridge")

Creamos 3 Bridge una para cada vlan anteriormente creada, tan solo definimos el nombre y le damos a OK.

Ahora estando en la misma ventana del __bridge__ cambiamos de pestaña yendo a _Ports_, y seguidamente le damos al símbolo más.
En la ventana que nos aparecerá, será donde definamos cada vlan con su correspondiente bridge y seguidamente ese mismo bridge con su interfaz, veamos el ejemplo con la vlan 10.

Primero en interfaz seleccionamos la vlan10_RH y en bridge seleccionamos bridge10_RRHH, aplicamos y luego ok.

![Bridge con vlan](/ImagenesPI/vlanbridge.PNG "Bridge con vlan")

Volvemos a darle al símbolo de más y ahora en interfaz seleccionamos ether2, y en bridge volvemos a elegir bridge10_RH.

![Bridge con interfaz](/ImagenesPI/etherbridge.PNG "Bridge con interfaz")

Ahora realizaremos los mismos pasos para unir el _ether3 con la vlan20_Secre y el Bridge20_Secretariado_, y después unimos el _eher4 con la vlan30_Admin y el bridge30_Administrativo_. Quedando al final todo como se muestra en la siguiente imagen.

![Todos los bridges creados](/ImagenesPI/bridgecreada.PNG "Todos los bridges creados")

Ahora con esta configuración cuando conectemos un PC al ether2 del switch corresponderá al vlan 10, si lo conectamos al ether3 corresponderá a la van 20, etc.

### Crear vlan solo en un Router

Si por el contrario en vez de tener un router y un switch solo tenemos un Router, el procedimiento sería muy parecido cambiando un par de cosas. Partiremos con la configuración que hicimos en la OVA de CHR que configuramos como Switch, con la diferencia de que ahora ether 1 no estará conectado a un Router sino a un ISP o en este caso al Router de casa, por lo tanto lo configuraremos como dhcp cliente.
Eso quiere decir que iremos al menú izquierdo y luego a __IP → Dhcp cliente__, le damos al símbolo más, seleccionamos el interface ether1 y le damos a ok, tal y como se ve en la siguiente imagen.

![Dhcp cliente de ether1](/ImagenesPI/dhcpcliente2.PNG "Dhcp cliente de ether1")

Como hemo dicho las vlan y los bridge ya están creadas porque partimos de la ova que antes configuramos, solo hemos cambiado de momento que el ether 1 reciba la ip por dhcp.

![Brige y vlan creadas](/ImagenesPI/bridgeyvlan.PNG "Brige y vlan creadas")

Ahora lo que debemos hacer es darle un direccionamiento a los bridge, para ello vamos al menú izquierdo, __IP → Addresses__ e iremos seleccionando cada uno de los Bridge y le daremos una IP, __OJO__ al asignarle la ip al bridge, hará que tanto la interfaz como la vlan puedan ver esa ip incluso si luego le configuramos dhcp.

![Dando ip al bridge](/ImagenesPI/ipbridge.PNG "Dando ip al bridge")

![Configuradas las ips para los 3 bridge](/ImagenesPI/ipsbridges.PNG "Configuradas las ips para los 3 bridge")

Ahora podemos dejar la configuración como está asignando una ip fija a cada equipo que se conecte a cada vlan, o podemos establecer un servicio dhcp a cada bridge, así que vamos a hacerlo.
Como siempre vamos al menú izquierdo, luego a __IP → Dhcp server__, y luego al símbolo más o al dhcp setup para que nos guíe el asistente, los pasos serán los mismos que los vistos anteriormente cuando configuramos el dhcp server con lo cual pasaremos a mostrar las capturas de imagenes de como seria la configuración.

![Interfaz del server](/ImagenesPI/dhcpbridge1.PNG "Interfaz del server")

![Red del dhcp](/ImagenesPI/dhcpbridge2.PNG "Red del dhcp")

![Puerta de enlace de la red](/ImagenesPI/dhcpbridge3.PNG "Puerta de enlace de la red")

![Rango de las ip asignadas por dhcp](/ImagenesPI/dhcpbridge4.PNG "Rango de las ip asignadas por dhcp")

![Servidores de DNS](/ImagenesPI/dhcpbridge5.PNG "Servidores de DNS")

![Tiempo de arrendamiento de la ip](/ImagenesPI/dhcpbridge6.PNG "Tiempo de arrendamiento de la ip")

Estos mismos pasos lo haremos para las otras 2 bridges. Quedando como resultado final tal y como se muestra en la siguiente imagen.

![Configurado el dhcp para las tres bridges](/ImagenesPI/dhcpbridgeconfig.PNG "Configurado el dhcp para las tres bridges")

Otro paso que debemos realizar es crear la regla de enmascaramiento en la NAT para el ether 1, de la misma forma que ya se realizó anteriormente, para no repetir solo pondremos las imágenes.

![Configurar enmascaramiento](/ImagenesPI/bridgenat.PNG "Configurar enmascaramiento")

![Configurar enmascaramiento](/ImagenesPI/bridgenat2.PNG "Configurar enmascaramiento")

Pero aún falta un detalle y es que si hacemos ping a la red de otra vlan por ejemplo a la vlan 20, nos responderá, y nosotros queremos que no se comuniquen entre ellas.

![Ping a otra Vlan](/ImagenesPI/ipvlanes.PNG "Ping a otra Vlan")

Tambien se puede ver la lista de rutas, en la cual vemos las rutas establecidas en el router.

![Lista de rutas del router](/ImagenesPI/listarutasvlan.PNG "Lista de rutas del router")

Por lo tanto debemos añadir un par de reglas en el Firewall, para que las vlans no se vean pero si salgan por internet.
Para ello en el menu izquierdo vamos a __IP → Firewall__, pestaña de _File Rules_ y le damos al símbolo de más. En donde definimos que todo lo que vaya desde la red 10.10.10.0 que pertenece a la vlan 10 con destino a la red 20.20.20.0 que pertenece a la vlan 20, le hacemos un drop

![Regla de firewall para vlan](/ImagenesPI/firewallvlan.PNG "Regla de firewall para vlan")

![Regla de firewall para vlan](/ImagenesPI/firewallvlan2.PNG "Regla de firewall para vlan")

Luego repetimos lo mismo pero a la inversa, todo lo que vaya de la vlan 20 a la vlan 10 le hacemos un drop.

![Regla de firewall inversa para vlan](/ImagenesPI/firewallvlan3.PNG "Regla de firewall inversa para vlan")

Entonces ya con esto podemos definir si queremos que alguna vlan si se vean entre ellas o que ninguna se vea entre ellas, en este caso haremos que sean completamente independientes y que no se vean entre ellas quedando todas las reglas como se muestra en la imagen.

![Reglas de firewall para vlanes creadas](/ImagenesPI/firewallvlanes.PNG "Reglas de firewall para vlanes creadas")

## Configurar DMZ y reglas de Firewall

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")

![Fase1](/ImagenesPI/FASE1.PNG "")












