# Seguridad

Según el diagrama siguiente vamos a explicar como configurar el __Port Knocking__.

![Topografía de red con gns3 para Port Knocking](ImagenesPI/PIFase3/topografia1.PNG "Topografía de red con gns3 para Port Knocking")

Partiremos de un router multiwan cuyas ip sean estáticas, y una red lan con servicio dhcp.

![Interfaces, direccionamiento de las mismas y las rutas](ImagenesPI/PIFase3/ipsconfiguradas.PNG "Interfaces, direccionamiento de las mismas y las rutas")

## Crear Lista de Interfaces

Lo primero que podemos hacer para facilitar el trabajo a la hora de crear las reglas de firewall es definir todas las redes WAN en una lista.
Para ello estando en la ventana de Interfaces vamos a la pestaña de _Interface List_.
Le damos al botón que dice __List__ y en la nueva ventana de _Interface Lists_ le damos al símbolo del (+), en la nueva ventana llamada _New Interface List_ vamos al aparatado de __Name__ donde definiremos el nombre que queramos en este caso _WAN_, aplicamos y ok. Con esto ya nos aparecerá el nombre definido en la lista de interfaces.

![Crear lista Wan](ImagenesPI/PIFase3/interfacelist.PNG "Crear lista Wan")

Ahora regresaremos a la ventana principal de _Interface List_ y le damos al (+), en esta nueva ventana lo que haremos será en el apartado _List_ elegir __WAN__ que fue el nombre que definimos antes, y en el apartado interface seleccionamos una de las diferentes interfaces wan que tengamos, en este caso como tengo tres debo agregar las tres interfaces a la lista wan.

![Agregar interfaces a la lista WAN](ImagenesPI/PIFase3/agregarinterfaceslista.PNG "Agregar interfaces a la lista WAN")

Al final nos debe quedar la lista como en la imagen siguiente.

![Lista wan creada](ImagenesPI/PIFase3/listwan.PNG "Lista wan creada")

## Port Knocking

Ahora pasamos a crear las reglas del port knocking, para ello vamos a __IP → Firewall__ y luego agregamos una regla dándole al (+).
En este ejemplo haremos que los golpes de puertos necesarios para poder entrar al router ya sea por (WinBox, SSH, Telenet, etc.) sean tres, tal y como se ve en la primera imagen de la topografía de red.
Lo primero en la pestaña _General_ vamos al apartado _Chain_ y le asignamos __Input__, luego vamos al apartado _Protocol_  y seleccionamos __6(tcp)__.
Al asignar un protocolo podremos definir los puertos de _Source y Destiny_ en este caso iremos al _Ds. Ports_ y escribimos 2000.
Lo siguiente será definir el interfaz wan de entrada en el apartado _In Interface_, el problema es que como en este ejemplo tenemos tres interfaces wan, se deberia crear tres reglas iguales para cada interfaz, por ello para simplificarlo y no crear tantas reglas, definimos con anterioridad una lista wan para todas las interfaces que van por wan.
Con lo cual nosotros vamos al apartado de _In Interface List_ y seleccionamos nuestra lista __WAN__ anteriormente creada, con esto tan solo hemos creado una regla definiendo a la entrada de todas las interfaces wan que tengamos asignada a esa lista, en vez de tener que crear una regla para cada interfaz de entrada por wan.

![Pestaña general regla 1](ImagenesPI/PIFase3/portknockinggeneral1.PNG "Pestaña general regla 1")

Ahora pasamos a la pestaña de _Action_, y seleccionamos la opción de __add src to address list__  en el apartado de _Action_.
Luego en _Address List_ definimos el nombre que nosotros queramos, en este ejemplo se ha usado __temporal__.
Y en el apartado de _Timeout_ definimos el tiempo que la ip va a estar en esta lista, en este ejemplo se a usado una duración de 5 minutos (00:05:00).
Aplicamos y OK.


![Pestaña action regla 1](ImagenesPI/PIFase3/portknockingaction1.PNG "Pestaña action regla 1")

Vamos a explicar que hace esta regla que acabamos de definir.
Lo que hemos hecho es definir que todo lo que entre al router por cualquier interfaz wan que se encuentre en la lista definida, y use el protocolo tcp y el puerto de destino 2000, lo agregue a una lista de direcciones que hemos llamado temporal, con una duración de 5 minutos, pasado ese tiempo se eliminará de la lista.
 
Ahora crearemos la siguiente regla que corresponderá con el segundo toque o golpeo de puerto.

![Pestaña general regla 2](ImagenesPI/PIFase3/portknockinggeneral2.PNG "Pestaña general regla 2")

Como se ve en la imagen anterior de momento lo único que se ha cambiado en la pestaña _General_ a sido el __puerto de Destino__ que ahora es _4000_.
Ahora pasamos a la pestaña _Advanced_, una vez aquí vamos al apartado de _Src. Address List_ y seleccionamos __temporal__.

![Pestaña advanced regla 2](ImagenesPI/PIFase3/portknockingadvanced2.PNG "Pestaña advanced regla 2")

Ahora pasamos a la pestaña _Action_ y definimos en el apartado de _Action_ __add src to address list__ en _Timeout_ podemos dejarlo de nuevo en 5 minutos eso es al gusto de cada uno, y en _Address List_ definimos un nuevo nombre, en este caso se a usado __permitido__.
Aplicamos y ok.

![Pestaña action regla 2](ImagenesPI/PIFase3/portknockingaction2.PNG "Pestaña action regla 2")

Con esta segunda regla lo que hemos definido es que todas las ip que se encuentren en la lista de temporal tendrán que hacer una conexión al puerto 4000, y pasarán a estar por 5 minutos en otra lista de ip a la que hemos denominado permitido.
 
Ahora pasamos a definir el tercer toque o golpeo de puerto tal y como venía en el diagrama de red.
Los pasos serán muy parecidos a los aplicados a la segunda regla, con lo que en la pestaña general solo cambiamos el _Dst. Port_ por 8000.

![Pestaña general regla 3](ImagenesPI/PIFase3/portknockinggeneral3.PNG "Pestaña general regla 3")

Seguidamente vamos a la pestaña de _Advanced_ y en el apartado de _Src. Address List_ definimos a la lista de ip que se encuentran en __permitido__ que son los que han realizado satisfactoriamente el primer y el segundo toque o golpeo de puerto.

![Pestaña advanced regla 3](ImagenesPI/PIFase3/portknockingadvanced3.PNG "Pestaña advanced regla 3")

Luego vamos a la pestaña de _Action_ donde volvemos a seleccionar __add src to address list__ en el apartado de _Action_, en _Address List__ nombramos a la nueva lista como seguro, y en _Timeout_ esta vez le daremos bastante más tiempo que lo usados anteriormente en este caso 1 hora (01:00:00) o 60 minutos (00:60:00) ambas valen. 

![Pestaña advanced regla 3](ImagenesPI/PIFase3/portknockingaction3.PNG "Pestaña advanced regla 3")

El motivo de añadir más tiempo a esta lista de ips es debido a que este será el último golpeo de puerto para poder acceder al router y por tanto esta ip tendrá acceso al router mientras se encuentre en esta lista, pasado ese tiempo será borrado y se le denegara el acceso al router, esto quiere decir que pasado 1 hora, la ip que haya podido acceder al router será eliminada de la lista y por tanto se le echara del router, debiendo conectarse nuevamente llamando a los tres toques o golpes de puerto que hemos definido.
Lo dos toques anteriores tenían un tiempo menor porque es el tiempo que le definimos para que llame al siguiente puerto, esto ayuda a que un cracker tenga poco tiempo para poder averiguar cual es el siguiente protocolo y puerto que debe usar para poder pasar la siguiente barrera de protección, puesto que pasado ese tiempo deberá empezara de nuevo llamando al primer puerto que se definió en la primera regla.
 
Ahora pasamos a definir la regla de aceptación.
En la pestaña _General_ tan solo definimos en _Chain_ el modo __Input_ y en _In. Interface List_ __WAN__.

![Pestaña general regla 4](ImagenesPI/PIFase3/portknockinggeneral4.PNG "Pestaña general regla 4")

Luego pasamos a la pestaña de _Advanced_ donde en _Src. address List_ definimos a las ip que se encuentren en la lista de __seguro__ ya que estas ip previamente han tenido que pasar las dos reglas anteriores que definimos.

![Pestaña advanced regla 4](ImagenesPI/PIFase3/portknockingadvanced4.PNG "Pestaña advanced regla 4")

Luego vamos a la pestaña de _Action_, donde el apartado de _Action_ lo dejamos en __accept__.
Aplicamos y OK.

![Pestaña action regla 4](ImagenesPI/PIFase3/portknockingaction4.PNG "Pestaña action regla 4")

Con esta regla hemos definido que todas aquellas ip, que hayan pasado las tres reglas anteriores tendrán permiso para entrar al router.
 
Por último crearemos una regla la cual impida que cualquier ip que no cumpla estas condiciones pueda entrar al router.
Para ello creamos una nueva regla, donde en la pestaña de _General_ definimos __Input__ en el apartado de _Chain_ y nuestra lista de __WAN__ en el apartado de _In Interface List_

![Pestaña general regla 5](ImagenesPI/PIFase3/portknockinggeneral5.PNG "Pestaña general regla 5")

Luego vamos a la pestaña de _Action_ y definimos __drop__ en el apartado de _Action_.
Aplicamos y OK.

![Pestaña action regla 5](ImagenesPI/PIFase3/portknockingaction5.PNG "Pestaña action regla 5")

Al final nos deben quedar las cinco reglas creadas como en la imagen siguiente.
Cumpliendo con lo definido en el diagrama de red.

![Reglas de Port Knocking creadas](ImagenesPI/PIFase3/portknockingreglas.PNG "Reglas de Port Knocking creadas")

## Crear Address List

A aparte del Port Knocking, podemos crear nosotros las address lists manualmente definiendo qué ip consideramos seguras para poder entrar en el router. 
Para ello tan solo tenemos que ir a la pestaña de _Address Lists_ dentro dentro de la ventana de __Firewall__ y una vez aquí darle al símbolo del (+).
En la ventana que nos aparece le damos un nombre a la lista de direcciones, agregamos la ip que vamos a permitir acceder al router y tambien si queremos podemos definir o no el tiempo que estará dentro de esta lista. 
Con esto podemos agregar todas las ip que queramos en una address list en concreto o crear una address list por ip, según más nos convenga.

![Creación manual de address list](ImagenesPI/PIFase3/addresslista.PNG "Creación manual de address list")

Una vez que tengamos definidas nuestras address list tan solo debemos crear una regla de firewall en la que permitamos el acceso a estas listas, definiendolo en la pestaña de _advanced_ y seleccionar las _addres list_ que deseamos de las creadas antes en el apartado de __Src. Address List__. 

![Crear regla de Firewall agregando nuestra Address List creada](ImagenesPI/PIFase3/exclusiones.PNG "Crear regla de Firewall agregando nuestra Address List creada")

![Regla creada](ImagenesPI/PIFase3/portknockingreglas2.PNG "Regla creada")

## Configurar Gmail en Mikrotik

Lo primero es saber la ip de nuestro servicio de correo de gmail, y el puerto, para eso podemos ir al siguiente enlace [SMTP gmail](https://support.google.com/a/answer/176600?hl=es).
Y nos fijamos en el siguiente apartado que se muestra en la imagen.

![SMTP GMAIL](ImagenesPI/PIFase3/gmail.PNG "SMTP GMAIL")

Lo más interesante de aquí son los puertos __(465 y 587)__ y la dirección del servidor smtp __smtp.gmail.com__.
Ahora pasamos a nuestro router mikotik, abrimos una terminal y hacemos ping a esa dirección de smtp, para saber la ip del mismo

![IP  de nuesto servicio SMTP](ImagenesPI/PIFase3/smtpip.PNG "IP de nuesto servicio SMTP")

Apuntamos la ip que nos aparece porque nos servirá más adelante.
Con esto ya tenemos el puerto y la ip.
Ahora para dar más seguridad iremos a la siguiente página [Contraseña de aplicaciones](https://support.google.com/accounts/answer/185833?hl=es-419) y seguiremos los pasos para crear contraseñas de aplicaciones.

![Contraseña de aplicaciones](ImagenesPI/PIFase3/contraseñaapp.PNG "Contraseña de aplicaciones")

Una vez dentro del apartado de contraseña de aplicación, vamos al apartado que dice _Seleccionar Aplicación_, marcamos otro, y escribimos por ejemplo _Router Mikortik_ o lo que cada uno vea mejor, y luego le damos a __Generar__.

![Creando contraseña de aplicaciones](ImagenesPI/PIFase3/contraseñaapp2.PNG "Creando contraseña de aplicaciones")

Apuntamos la contraseña que nos dan de 16 dígitos después de darle al botón de generar.

![Contraseña dada por Gmail](ImagenesPI/PIFase3/contraseñaapp3.PNG "Contraseña dada por Gmail")

Ahora tenemos todo lo necesario. Así que pasamos a nuestro router Mikrotik.
Una vez en el router vamos al menú izquierdo donde dice __Tools → Email__
y en la ventana que nos aparece escribimos lo siguiente en cada apartado.
- __Server:__ ip de nuestro servicio de smtp
- __Port:__ el puerto que vamos a usar
- __Start Tls:__ hace referencia al transporte criptográfico de seguridad
- __From:__ nuestro correo electrónico
- __User:__ el usuario al cual va dirigido, funciona mejor si dejamos nuestro correo al igual que en _from_
- __Password:__ la contraseña que nos dio google en el apartado de contraseña de aplicaciones. Aunque también se puede usar la propia contraseña de nuestro correo electronico, pero de esta manera no es tan segura ni fiable.
Aplicamos y ok.

![Configuración de correo en Mikrotik](ImagenesPI/PIFase3/gmailmikrotik.PNG "Configuración de correo en Mikrotik")

Un dato a tener en cuenta para que funcione correctamene el envio de correos es tener activo nuestro servicio SNTP (Server Network Time Protocol) activado, para ello vamos a __System → SNTP client__.
En la ventana que nos aparece marcamos la casilla de _Enabled_, y en _Primary NTP Server_ escribimos __time.google.com__, luego le damos al botón de Apply y automáticamente nos reconocerá la ip de este servicio (siempre que tengamos bien configurado nuestro DNS), rellenando automáticamente lo necesario, así que le damos a OK.

![Configurar NTP](ImagenesPI/PIFase3/sntpcliente.PNG "Configurar NTP")

## Configurar que nos lleguen errores y dhcp por correo

Una vez configurado nuestro servicio de correo en Mikrotik vamos a definir que queremos que nos envíe por email, para ellos vamos a __System → Logging__
Una vez aquí vamos primero a la pestaña de _Actions_, y le damos al simbolo del (+).

![Logging pestala Action](ImagenesPI/PIFase3/gmailerror.PNG "Logging pestala Action")

En la ventanita que nos aparece, definimos el nombre de la acción en _Name_ por ejemplo __email__, definimos de que tipo será en _Type_ el cual será __email__, activamos la casilla _Start TLS_ y en el apartado _Email_ definimos el correo al cual será enviado, aplicamos y ok.

![Creando la acción de email](ImagenesPI/PIFase3/gmailerror2.PNG "Creando la acción de email")

Ya nos deberá aparecer nuestra acción creada.

![Acción email creada](ImagenesPI/PIFase3/gmailerror3.PNG "Acción email creada")

Acto seguido vamos a la pestaña de _Rules_ y le damos al símbolo del (+)

![Logging pestala Rules](ImagenesPI/PIFase3/gmailerror4.PNG "Logging pestala Rules")

En la ventana que nos aparece en donde dice _Action_ seleccionamos la acción que previamente creamos como __email__, y en _Topics_ nos saldrá un desplegable con una gran cantidad de opciones donde podemos elegir la que deseemos, en este caso seleccionaremos __error__, luego le damos a aplicar y ok.

![Creando la regla de error por correo](ImagenesPI/PIFase3/gmailerror5.PNG "Creando la regla de error por correo")

Con esta regla cada vez que se produzca un error en el router, nos será notificado al correo que definimos en la _Action_, de esta manera podemos crear todas las reglas que queramos definiendo qué aspectos queremos que se nos comuniquen por correo. 
En este ejemplo aparte de los errores se creó otra regla para el dhcp como se muestra a continuación.

![Reglas de error y dhcp creadas](ImagenesPI/PIFase3/gmailerror6.PNG "Reglas de error y dhcp creadas")

## Configurar que nos lleguen archivox Backup por correo

También podemos configurar el router Mikrotik para que nos envíe automáticamente cada cierto tiempo un archivo de respaldo de la configuración del router.
Para ello usaremos los siguientes scripts.

Backup Binario
~~~
/system script add name=respaldo_binario source={/system backup save name=([/system identity get name] . "-" . \
[:pick [/system clock get date] 7 11] . [:pick [/system clock get date] 0 3] . [:pick [/system clock get date] 4 6]); \
/tool e-mail send to="youremail@yourdomain.com" subject=([/system identity get name] . " Backup " . \
[/system clock get date]) file=([/system identity get name] . "-" . [:pick [/system clock get date] 7 11] . \
[:pick [/system clock get date] 0 3] . [:pick [/system clock get date] 4 6] . ".backup"); :delay 10; \
/file rem [/file find name=([/system identity get name] . "-" . [:pick [/system clock get date] 7 11] . \
[:pick [/system clock get date] 0 3] . [:pick [/system clock get date] 4 6] . ".backup")]; \
:log info ("System Backup emailed at " . [/sys cl get time] . " " . [/sys cl get date])}
~~~

Backup Export
~~~
/system script add name=respaldo_export source={/export file=([/system identity get name] . "-" . \
[:pick [/system clock get date] 7 11] . [:pick [/system clock get date] 0 3] . [:pick [/system clock get date] 4 6]); \
/tool e-mail send to="backup@YOURDOMAIN.com" subject=([/system identity get name] . " Backup " . \
[/system clock get date]) file=([/system identity get name] . "-" . [:pick [/system clock get date] 7 11] . \
[:pick [/system clock get date] 0 3] . [:pick [/system clock get date] 4 6] . ".rsc"); :delay 10; \
/file rem [/file find name=([/system identity get name] . "-" . [:pick [/system clock get date] 7 11] . \
[:pick [/system clock get date] 0 3] . [:pick [/system clock get date] 4 6] . ".rsc")]; \
:log info ("System Backup emailed at " . [/sys cl get time] . " " . [/sys cl get date])}
~~~

Cada script corresponde a uno de los dos tipos de respaldo que puede crear Mikrotik.
 
- El __Binario__ es un respaldo total de la configuración del router, el cual guarda tanto los usuario como las contraseñas del mismo, está pensado para usarse en el mismo router cuando haya algún tipo de error, el tipo de texto esta en binario con lo cual si intentamos abrirlo está todo cifrado, siendo ilegible y solo reconocible por dispositivos mikrotik.
- El __Export__ puede ser un respaldo total o parcial de la configuración del router, el cual es un script de texto plano que no guarda ni los usuarios ni las contraseñas.

Pues una vez explicado eso, abrimos una terminal desde el router y copiamos uno de los dos script, lo pegamos en la terminal y le damos a _Intro_ para ejecutarlo, luego realizamos el mismo procedimiento con el otros script.

![Insertando ambos script en la terminal del route](ImagenesPI/PIFase3/respaldo2.PNG "Insertando ambos script en la terminal del router")

Una vez ejecutado ambos script en la terminal vamos al menú izquierdo y luego a __System → Script__, en la ventana de _Script List_ nos deberá de aparecer dos archivos los cuales corresponden a los dos script ejecutados antes.

![Script en la lista de script](ImagenesPI/PIFase3/respaldo3.PNG "Script en la lista de script")

Ahora deberemos de hacer algunas modificaciones a los dos script, hacemos doble clic por ejemplo en el _respaldobinario_ y buscamos la siguiente línea (/tool e-mail send to="youremail@yourdomain.com") en el cuadro de texto de abajo 
de la ventana, en esa línea debemos modificar lo que está entre comillas y agregar el correo al cual queremos que llegue el backup. 
También podemos fijarnos en la siguiente linea (subject=([/system identity get name]), esta línea hace referencia al nombre que le hayamos definido al router en __System Identity__, por lo tanto es bueno definir un nombre para cada router, no solo para identificarlo, sino para cuando nos llegue este correo sepamos a qué router hace referencia.

![Modificar el correo en los script](ImagenesPI/PIFase3/respaldo4.PNG "Modificar el correo en los script")

Una vez cambiado el correo en ambos script pasamos a configurar el envío programado de los mismos, para ellos vamos al menú izquierdo, __System → Scheduler__ y luego al símbolo del (+).

![Ventana Scheduler](ImagenesPI/PIFase3/respaldo5.PNG "Ventana Scheduler")

En la nueva ventana que nos aparece definiremos cuando se inicia y cada cuanto tiempo se ejecuta el script.
Por tanto en cada apartado configuraremos lo siguiente:
- __Name:__ Damos el nombre que queramos para definirlo
- __Start Date:__ Es la fecha en la cual empezará a ejecutarse esta tarea
- __Start Time:__ Es la hora de inicio en la cual emepazará a ejecutarse la tarea
- __Interval:__ Es el intervalo de tiempo, para que se ejecute nuevamente la tarea, es este ejemplo se a definido que sea una vez al dia es decir cada 24 horas
- __Owner:__ Hace referencia al propietario de la creación de la tarea, este aparece automáticamente al darle al botón _Apply_
- Las __Policies__ las podemos dejar por defecto todas marcadas
En el cuadro de texto de abajo escribimos el nombre del uno de los script que previamente creamos, para definir qué será eso lo que deberá ejecutar en el tiempo definido
Los demas campos se rellenan solos al darle al botón de _Apply_ le damos al OK para terminar.
Seguidamente creamos otra regla con el mismo tiemppo o distinto segun prefiramos para el otro script de respaldo el de __respaldoexport__.

![Creando la tarea programada para el primer script](ImagenesPI/PIFase3/respaldo6.PNG "Creando la tarea programada para el primer script")

Al final nos quedarán ambas reglas creadas como en la imagen siguiente.

![Tareas programadas creadas](ImagenesPI/PIFase3/respaldo7.PNG "Tareas programadas creadas")

## Layer 7

Vamos a configurar nuestro router mikrotik con layer 7 para filtrar ciertos paquetes y de esta manera bloquear el acceso a ciertas páginas web.

Nuestro primer paso será ir a __IP → Firewall__ y luego a la pestaña de _Layer 7 Protocols_, una vez aquí le damos al símbolo (+) y en la ventana que nos aparece definimos el nombre que nosotros queramos en el aparatado _Name_ y en el apartado _Regexp_ será donde insertemos una expresión regular.
En este caso se insertó la siguiente expresion regular:

~~~
^.+(youporn.com|pornhub.com|xvideos.com|xvideos|pornstars|youtube.com|youtu.be|googlevideo.com|facebook).*$
~~~

La idea será bloquear las paginas que vienen definidas en la expresión regular.
Aplicamos y Ok.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

Nuestro siguiente paso será estando en la ventana de _Firewall_ ir a la pestaña de _Mangle_. 
Una vez aquí crearemos dos reglas.
La primera regla será para marcar las conexiones que van por DNS y que  cumplan el protocolo de L7 antes definido.
Para ello realizaremos lo siguiente, en la ventana que nos aparece al crear una nueva regla estando en la pestaña _General_  seleccionamos __prerouting__ en el apartado _chain_, en el apartado de _Protocol_ seleccionamos __UDP__,
en el apartado de _Dst.Port_ escribimos __53__ y en el apartado de _Connection Mark_ seleccionamos __no-mark__.
Ahora pasamos a la pestaña de Advanced, aquie solo debemos ir al aparatado de _Layer 7 Protocol_ y selecionar el protocolo que definimos anteriormente en Layer7, si tenemos varios pues seleccionamos el que nos interese, ya que podemos crear todos los protocolos de layer7 que queramos según nuestras necesidades.
Luego pasamos a la pestaña de _Action_, en ella seleccionamos __mark connection__ en el apartado de _Action_, en _New Connection Mark_  definimos un nombre que nosotros queramos y el _Passthough_ lo dejamos marcado para que pase a la siguiente regla. Aplicamos y OK.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

Ahora crearemos la otra regla para marcar los paquetes que provengan de las conexiones marcadas de la anterior regla.
Estando en la pestaña _General_ tan solo seleccionamos __prerouting__ en el apartado de _chain_ y en _Connection Mark_ seleccionamos el nombre que definimos en la anterior regla para las conexiones marcadas.
Luego pasamos a la pestaña _Action_, en esta seleccionamos __mark packet__ en el apartado de _Action_, definimos un nombre que nosotros queramos en el apartado de _New Packet Mark_ y marcamos el _Passthrough_.Aplicamos y ok.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

Ahora pasamos a crear las reglas de Firewall, entonces en la ventana de Firewall vamos a la pestaña de _Filter Rules_  y creamos dos reglas cuya _Action_ será __drop__, y en la pestaña _General_ en el apartado de _Packet Market_ seleccionamos el nombre que definimos en la segunda regla de mangle que usamos para definir los paquetes marcados.
La única diferencia de estas dos reglas será en el apartado _Chain_ donde uno hará referencia a la cadena __forward__ para prohibir las peticiones que coincidan con la definición de L7 señalada , y la otra regla hace referencia a la cadena  __Input__ porque en casos normales el router hace  las veces de DNS para la red.

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

![Fase2](ImagenesPI/PIFase3/Fase2.PNG "")

