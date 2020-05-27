# Listado de Pruebas

## Fase 1

- Comprobar que la interfaz WAN del router que irá como cliente de dhcp, reciba ip y tenga conectividad.
- Comprobar que los equipos reciben ip dinámica para aquellos que tengan un servicio de dhcp, y ver que tengan conectividad al exterior.
- Comprobar que los equipos con ip estáticas, tienen conectividad con otros equipos y salida al exterior.
- Comprobar que las reglas de Nat y de firewall están bien configuradas haciendo pruebas de conectividad entre los equipos según como se haya configurado la red.
  - La DMZ debe tener salida al exterior y conexión con el administrador de los servidores de la LAN2, con el resto de equipos de la LAN2 y de la LAN1 no deben tener conexión. 
  - La LAN2 tendrá salida al exterior, dentro de esta habrá dos administradores uno para la DMZ donde solo este tendra conexion a ella incluso conexión por ssh, pero no tendra conexion a la LAN1, y el otro administrador de la LAN2  que se encargará de las Vlans deberá tener conexión a estas pero no a la DMZ.
  - Las Vlans no tendrán salida al exterior ni a la DMZ pero sí tendrá conexión con el administrador de LAN2 que se encarga de las Vlans.
- Comprobar que los equipos de cada vlan reciba ip dinámica y que no tengan conexión entre ellas mismas.
- Comprobar que se crea el túnel por IPsec, se establece la conexión entre los vecinos, y hay conectividad entre ellos y los equipos definidos de cada red.

## Fase 2

- Comprobar el funcionamiento de Failover, haciendo que uno de los ISP se apague y ver si salta la ruta de respaldo para que mantenga la conectividad, luego volver a encender el ISP que antes hemos apagado para comprobar que cambia de nuevo la ruta de respaldo a como estaba al principio.
- Comprobar el funcionamiento del vrrp, para ello se desconectara el router maestro, y se comprobará que en el equipo cliente sigue teniendo conexión a internet y que salta el vrrp que da al router backup, luego volvemos a encender el router maestro, y comprobamos que vuelve a cambiar la vrrp del backup al maestro.
- Comprobar que la velocidad de subida y bajada cambia según la regla aplicada, para ellos se hará dos test a un mismo equipo, uno antes de aplicar la regla, y luego otro test después de aplicar las reglas de ancho de banda.
- Comprobar el balanceo de carga para la DMZ, para ello usaremos una ubuntu desktop, abriremos dos pestañas de internet donde se estén viendo videos en ambos, se comprobará que la ip corresponde a un equipo de la DMZ, y por winbox comprobaremos si el tráfico fluye por ambos interfaces WAN , y la interfaz que da acceso a la DMZ deberá dar la suma relativa de ambos proveedores de internet.

## Fase 3

- Comprobar el funcionamiento de Port Knocking, para ello estaremos conestados al router por winbox e intentaremos entrar por ssh desde la maquina anfitriona, luego iremos entrando segun los puertos asignados para ver como se va agregando la ip de la maquina enfitriona a la Address List permitidas asta poder entrar en el router.
- Comprobar el funcionamiento del correo de mikrotik por gmail, realizando un envío del mismo y comprobar que nos ha llegado el correo.
- Comprobar que nos llegan los errores y problemas relacionados con dhcp por correo.
- Comprobar que el programa para que lleguen correos automaticamente segun el tiempo especificado funciona correctamente.
- Comprobar que suricata funciona, creando una regla manualmente, que recoja todos los icmp, y ver si lo capta los archivos log.

