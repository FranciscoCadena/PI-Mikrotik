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


