# Suricata como IDS junto a Mikrotik

## Instalación de Suricata desde repositorio

Suricata ya viene por defecto en los repositorios de Ubuntu pero es posible que sea una versión antigua, por ello para asegurarnos de que se instala la última versión añadiremos el siguiente __PPA(Personal Package Archive) al repositorio__ con el siguiente comando.
~~~
sudo add-apt-repository ppa:oisf/suricata-stable
~~~

![Fase2](ImagenesPI/Suricata/repositorio.PNG "")

![Fase2](ImagenesPI/Suricata/repositorio2.PNG "")

Acto seguido actualizamos nuestro equipo con.
~~~
sudo apt update
~~~

![Fase2](ImagenesPI/Suricata/update.PNG "")

Y pasamos a instalarlo con.
~~~
sudo apt install suricata jq
~~~

![Fase2](ImagenesPI/Suricata/instalar.PNG "")

Con el comando anterior instalamos tanto suricata como la herramienta _jq_ la cual nos servirá para trabajar con las salidas que nos aporta el archivo __EVE.json__ del _log_.
 
Una vez instalado suricata podemos confirmarlo usando el siguiente comando para ver en qué estado se encuentra.
~~~
sudo systemctl status suricata
~~~

![Fase2](ImagenesPI/Suricata/status.PNG "")

Y tambien podemos usar el siguiente comando que nos dara informacion de todo lo instalado, tanto herramientas, como librerías, directorios de instalación, etc.
~~~
sudo suricata --build-info
~~~ 

![Fase2](ImagenesPI/Suricata/build.PNG "")

Lo siguiente que haremos será comprobar cuál es nuestra red y nuestra interfaz, para ello podemos usar el comando __ip a__ o cualquier otro que deseemos que nos de la información que deseamos.

![Fase2](ImagenesPI/Suricata/ip.PNG "")

Una vez que conocemos estos datos iremos al archivo de configuración de suricata, que se encuentra en __/etc/suricata/suricata.yaml__, una vez dentro buscamos el _HOME_NET_ y especificamos la red que tenemos, por ejemplo si tenemos la siguiente ip 192.168.0.10, definiremos en el Home_Net que nuestra red se encuentra en 192.168.0.0/24 o podemos directamente definir la ip de nuestra interfaz siempre que esta sea fija.

![Fase2](ImagenesPI/Suricata/suricatayaml.PNG "")

~~~
af-packet:
- interface: enp0s3
   cluster-id: 99
   cluster-type: cluster_flow
   defrag: yes
   use-mmap: yes
   tpacket-v3: yes
~~~

Seguidamente buscaremos en el documento todos los interfaces _eth0_ y los sustituiremos por el nuestro, una vez terminado guardamos, y pasaremos al siguiente fichero de texto __/etc/default/suricata__ donde buscaremos todos los _eth0_ y los sustituiremos por nuestra interfaz, al terminar guardamos y salimos.

![Fase2](ImagenesPI/Suricata/default.PNG "")

Nuestro siguiente paso será instalar la herramienta __Suricata-update__ la cual es muy importante en suricata debido a que nos proporciona las _reglas_ para las _alertas_ que usará suricata, estas vienen firmadas de diversas fuentes, de esta manera las podemos descargar y tener siempre actualizadas.
Para ello tan solo debemos ejecutar el siguiente comando.
~~~
sudo suricata-update
~~~

![Fase2](ImagenesPI/Suricata/suricata-update.PNG "")

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

![Fase2](ImagenesPI/Suricata/suricatalog.PNG "")

La linea mas importante de todo lo que nos muestra es la siguiente.
<Notice> - all 4 packet processing threads, 4 management threads initialized, engine started.
La cantidad de __threads(hilos)__ dependera del sistema y su configuración.  
 
### OPCIONAL
Una vez que comprobamos que suricata está instalado y corriendo, vamos a actualizar las fuentes, para ello ejecutamos.
~~~
sudo suricata-update update-sources
~~~

![Fase2](ImagenesPI/Suricata/updatesources.PNG "")

Luego vemos una lista de todas las fuentes disponibles con.
~~~
sudo suricata-update list-sources
~~~

![Fase2](ImagenesPI/Suricata/listsources.PNG "")

Una vez que veamos el que nos interesa tan solo debemos activarlo, en este ejemplo se activa el _OISF_, usando el __name__ completo que vimos en la lista anterior. 
~~~
sudo suricata-update enable-source oisf/trafficid
~~~

![Fase2](ImagenesPI/Suricata/oisf.PNG "")

Y luego actualizamos nuestras reglas.
~~~
sudo suricata-update
~~~
Ahora debemos reiniciar suricata con.
~~~
sudo systemctl restart suricata
~~~ 
Comprobamos las listas activas de la fuente con.
~~~
sudo suricata-update list-enabled-sources
~~~ 

![Fase2](ImagenesPI/Suricata/listenablesources.PNG "")

Como se dijo antes todas las reglas van a un archivo llamado _suricata.rules_ que se encuentra en la ruta _/var/lib/suricata_, para comprobar que suricata está leyendo correctamente de esta dirección vamos al archivo de configuración el cual se encuentra en _/etc/suricata/suricata.yaml_ y buscamos __rule-files__ donde deberá estar el archivo _suricata.rules_ y en __default-rule-path__ debera estar la ruta _/var/lib7suricata/rules_.

![Fase2](ImagenesPI/Suricata/suricatayamlreglas.PNG "") 
 
Para poder definir qué reglas queremos tener activas y cuales desactivadas lo normal es modificar el archivo __suricata.rules__,  para poder entrar al directorio donde se encuentra _/var/lib/suricata_, debemos ser root.

![Fase2](ImagenesPI/Suricata/varlibsuricata.PNG "") 

Otra forma que no viene por defecto al instalar el suricata-update es teniendo varios archivos en la ruta _/etc/suricata_ para definir lo que queremos hacer con cada regla, esos archivos son los siguiente.
- __/etc/suricata/enable.conf__, activar reglas
- __/etc/suricata/disable.conf__, desactivar reglas
- __/etc/suricata/drop.conf__, eliminar reglas
- __/etc/suricata/modify.conf__, modificar reglas
Podemos ejecutar el siguiente comando para tener un ejemplo de cada uno de ellos con. 
~~~
suricata-update --dump-sample-configs
~~~

![Fase2](ImagenesPI/Suricata/etcsuricata.PNG "") 

Aparte de tener estos archivos debemos especificarlos en el archivo de configuración de suricata el __suricata.yaml__, para poder ver un ejemplo mas claro de esto podemos visitar la siguiente pagina [suricata-update](https://suricata-update.readthedocs.io/en/latest/update.html#example-configuration-files).
De todas formas el futuro del uso de suricata es realizando todo desde el archivo _suricata.rules_.
 
