# Suricata como IDS junto a Mikrotik

## Instalación de Suricata desde repositorio

Suricata ya viene por defecto en los repositorios de Ubuntu pero es posible que sea una versión antigua, por ello para asegurarnos de que se instala la última versión añadiremos el siguiente __PPA(Personal Package Archive) al repositorio__ con el siguiente comando.
~~~
sudo add-apt-repository ppa:oisf/suricata-stable
~~~
Acto seguido actualizamos nuestro equipo con.
~~~
sudo apt update
~~~
Y pasamos a instalarlo con.
~~~
sudo apt install suricata jq
~~~
Con el comando anterior instalamos tanto suricata como la herramienta _jq_ la cual nos servirá para trabajar con las salidas que nos aporta el archivo __EVE.json__ del _log_.
 
Una vez instalado suricata podemos confirmarlo usando el siguiente comando para ver en qué estado se encuentra.
~~~
sudo systemctl status suricata
~~~
Y tambien podemos usar el siguiente comando que nos dara informacion de todo lo instalado, tanto herramientas, como librerías.
~~~
sudo suricata --build-info
~~~ 
Lo siguiente que haremos será comprobar cuál es nuestra red y nuestra interfaz, para ello podemos usar el comando __ip a__ o cualquier otro que deseemos que nos de la información que deseamos.
 
Una vez que conocemos estos datos iremos al archivo de configuración de suricata, que se encuentra en __/etc/suricata/suricata.yaml__, una vez dentro buscamos el _HOME_NET_ y especificamos la red que tenemos, por ejemplo si tenemos la siguiente ip 192.168.0.10, definiremos en el Home_Net que nuestra red se encuentra en 192.168.0.0/24 o podemos directamente definir la ip de nuestra interfaz siempre que esta sea fija.
 
Seguidamente buscaremos en el documento todos los interfaces _eth0_ y los sustituiremos por el nuestro, una vez terminado guardamos, y pasaremos al siguiente fichero de texto __/etc/default/suricata__ donde buscaremos todos los _eth0_ y los sustituiremos por nuestra interfaz, al terminar guardamos y salimos.
 
Nuestro siguiente paso será instalar la herramienta __Suricata-update__ la cual es muy importante en suricata debido a que nos proporciona las _reglas_ para las _alertas_ que usará suricata, estas vienen firmadas de diversas fuentes, de esta manera las podemos descargar y tener siempre actualizadas.
Para ello tan solo debemos ejecutar el siguiente comando.
~~~
sudo suricata-update
~~~
Este comando funciona con versiones de suricata del 4.1 en adelante si son más antiguas habrá que instalarlo con los siguientes comandos:
~~~
apt install python-pip
pip install pyyaml
pip install https://github.com/OISF/suricata-update/archive/master.zip
pip install --pre --upgrade suricata-update
~~~
Todas las reglas se instalarán en la ruta por defecto __/var/lib/suricata/rules__, dentro del archivo __suricata.rules__.
 
Ahora reiniciamos suricata con.
~~~
sudo systemctl restart suricata
~~~ 
Luego nos aseguramos de que suricata está corriendo comprobando su fichero de _log_, usando el comando. 
~~~
sudo tail /var/log/suricata/suricata.log
~~~
La linea mas importante de todo lo que nos muestra es la siguiente.
<Notice> - all 4 packet processing threads, 4 management threads initialized, engine started.
La cantidad de __threads(hilos)__ dependera del sistema y su configuración.  
