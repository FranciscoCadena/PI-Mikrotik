# Definiciones

## Drop vs Reject

Para paquetes que vengan del exterior lo recomendable es usar _DROP_, para los paquetes dentro de una red Local o privada puede usarse _Reject_.
La opción REJECT envía un mensaje ICMPP avisando que fue rechazada, sin embargo esto puede ser usado por otras personas que realicen un ataque DDOS (denegación de servicios). Muchos de estos ataques son falsificaciones, que se aprovechan de la ventaja de REJECT vs DROP. 

## Port knocking

El bloqueo de puertos es un método que permite el acceso al enrutador solo después de recibir intentos de conexión secuenciados en un conjunto de puertos cerrados previamente especificados. Una vez que se recibe la secuencia correcta de los intentos de conexión, el RouterOS agrega dinámicamente una IP de origen del host a la lista de direcciones permitidas y podrá conectar su enrutador.

Mejora la seguridad de nuestro dispositivo y minimizar el riesgo de intentos de piratería en protocolos como SSH, Telnet, Winbox, etc.

## Expresión regular

Las expresiones regulares son patrones utilizados para encontrar una determinada combinación de caracteres dentro de una cadena de texto. Las expresiones regulares proporcionan una manera muy flexible de buscar o reconocer cadenas de texto. Por ejemplo, el grupo formado por las cadenas Handel, Händel y Haendel se describe con el patrón "H(a|ä|ae)ndel".

## IPsec

Internet Protocol Security (IPsec) es un conjunto de protocolos definidos por Internet Engineering Task Force (IETF) para asegurar el intercambio de paquetes a través de redes IP / IPv6 desprotegidas como Internet.El conjunto de protocolos IPsec se puede dividir en los siguientes grupos:

- __Protocolos de intercambio de claves de Internet (IKE)__. Genera y distribuye dinámicamente claves criptográficas para AH y ESP.

Es un protocolo que proporciona material de claves autenticado para el marco de la Asociación de seguridad de Internet y el Protocolo de administración de claves (ISAKMP). Existen otros esquemas de intercambio de claves que funcionan con ISAKMP, pero IKE es el más utilizado. Juntos proporcionan medios para la autenticación de hosts y la gestión automática de asociaciones de seguridad (SA).

- __Encabezado de autenticación (AH) RFC 4302__

Es un protocolo que proporciona autenticación de todo o parte del contenido de un datagrama mediante la adición de un encabezado que se calcula en función de los valores en el datagrama. Las partes del datagrama que se usan para el cálculo y la ubicación del encabezado dependen de si se usa el modo túnel o transporte.

La presencia del encabezado AH permite verificar la integridad del mensaje, pero no lo encripta. Por lo tanto, AH proporciona autenticación pero no privacidad. Otro protocolo (ESP) se considera superior, proporciona privacidad de datos y también su propio método de autenticación.

- __Carga de seguridad de encapsulación (ESP) RFC 4303__

Utiliza cifrado de clave compartida para proporcionar privacidad de datos. ESP también admite su propio esquema de autenticación como el utilizado en AH.

ESP empaqueta sus campos de una manera muy diferente a AH. En lugar de tener solo un encabezado, divide sus campos en tres componentes:

- _Encabezado ESP_: viene antes de los datos cifrados y su ubicación depende de si ESP se usa en modo de transporte o en túnel.
- _ESP Trailer_: esta sección se coloca después de los datos cifrados. Contiene relleno que se utiliza para alinear los datos cifrados.
- _Datos de autenticación ESP_: este campo contiene un valor de comprobación de integridad (ICV), calculado de manera similar a cómo funciona el protocolo AH, para cuando se utiliza la función de autenticación opcional de ESP.

## Datagramas

Paquete sencillo enrutado en una red sin reconocimiento.

Un __datagrama__ es un fragmento de paquete que es enviado con la suficiente información como para que la red pueda simplemente encaminar el fragmento hacia el ordenador receptor, de manera independiente a los fragmentos restantes. Esto puede provocar una recomposición desordenada o incompleta del paquete en el ordenador destino. Su estructura se compone de _cabecera y datos_.

## TZSP

TaZmen Sniffer Protocol (TZSP) es un protocolo de encapsulación utilizado para envolver otros protocolos. Se usa comúnmente para envolver paquetes inalámbricos 802.11 para admitir sistemas de detección de intrusiones (IDS) , seguimiento inalámbrico u otras aplicaciones inalámbricas.
Usa el protocolo _UDP_ y el puerto 37008.

## PCAP

Es una interfaz de una aplicación de programación para captura de paquetes. La implementación del pcap para sistemas basados en Unix se conoce como libpcap; el port para Windows del libpcap recibe el nombre de WinPcap.

Algunos programas que usan pcap:
- Snort
- Suricata
- Nmap
- tcpdump


