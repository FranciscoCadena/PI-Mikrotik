# Seguridad

Según el diagrama siguiente vamos a explicar como configurar el __Port Knocking__.

![Topografía de red con gns3 para Port Knocking](ImagenesPI/PIFase3/topografia1.PNG "Topografía de red con gns3 para Port Knocking")

Partiremos de un router multiwan cuyas ip sean estáticas, y una red lan con servicio dhcp.

![Interfaces, direccionamiento de las mismas y las rutas](ImagenesPI/PIFase3/ipsconfiguradas.PNG "Interfaces, direccionamiento de las mismas y las rutas")

Lo primero que podemos hacer para facilitar el trabajo a la hora de crear las reglas de firewall es definir todas las redes WAN en una lista.
Para ello estando en la ventana de Interfaces vamos a la pestaña de _Interface List_.
Le damos al botón que dice __List__ y en la nueva ventana de _Interface Lists_ le damos al símbolo del (+), en la nueva ventana llamada _New Interface List_ vamos al aparatado de __Name__ donde definiremos el nombre que queramos en este caso _WAN_, aplicamos y ok. Con esto ya nos aparecerá el nombre definido en la lista de interfaces.

![Crear lista Wan](ImagenesPI/PIFase3/interfacelist.PNG "Crear lista Wan")

Ahora regresaremos a la ventana principal de _Interface List_ y le damos al (+), en esta nueva ventana lo que haremos será en el apartado _List_ elegir __WAN__ que fue el nombre que definimos antes, y en el apartado interface seleccionamos una de las diferentes interfaces wan que tengamos, en este caso como tengo tres debo agregar las tres interfaces a la lista wan.

![Agregar interfaces a la lista WAN](ImagenesPI/PIFase3/agregarinterfaceslista.PNG "Agregar interfaces a la lista WAN")

Al final nos debe quedar la lista como en la imagen siguiente.

![Lista wan creada](ImagenesPI/PIFase3/listwan.PNG "Lista wan creada")

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

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")

![Fase2](imagenesPI/PIFase3/Fase2.PNG "")
