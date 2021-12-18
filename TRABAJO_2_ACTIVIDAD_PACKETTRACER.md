author: Raúl Cabrera Garcia
summary: Guía técnica configurar OSPFv2 con autenticación MD5
id: TRABAJO_2_PACKET_TRACER
categories: codelab,markdown
environments: Web
status: Published
feedback link: Enlace github para los issue

# Guía técnica para configurar OSPFv2 con autenticación MD5 e interfaces pasivas.

## Página 1
Duration: 00:02:00

### Mostramos el esquema de red que usaremos durante la guía:

![Foto0_PACKET2](img/foto0.jpg)

Negative
: TENEMOS 4 ZONAS:

* Zona Roja: 2 PC's conectados al Switch0 y este conectado al Router1. A su vez el PC5 estará después conectado a una *VLAN* llamada *administradores*.
* Zona amarilla: 2 PC's conectados al Switch1 y este conectado al Router2.
* Zona Amarilla: 3 PC's conectados al Switch2 y este conectado al Router3. A su vez el PC6 estará conectado a una *VLAN* llamada *administradores*.
* Zona azul: Estan interconectados ***Router1, Router2 y Router3***, mediantes cables seriales, que componen el ***AREA 0*** o bakcbone del protoclo **OSPFv2**, veremos mas adelante.

Positive
: El objetivo de está guía técnica es poder configurar con **OSPFv2** y autenticación **MD5** las diferentes redes que componen el supuesto. Indicando también que interfaces queremos que sean pasiva como metódo de seguridad.

**INFORMACIÓN DE CONFIGURACIÓN E INTERFACES DE LOS DISTINTOS DISPOSITIVOS**


* PC1
	* **IP**: 192.168.2.11
	* **Máscara de subred**: 255.255.255.0
	* **Gateway**: 192.168.2.1
	* **Interfaz a la que se conecta**: FastEthernet0/1 del Switch0

* PC2
	* **IP**: 192.168.2.12
	* **Máscara de subred**: 255.255.255.0
	* **Gateway**: 192.168.2.1
	* **Interfaz a la que se conecta**: FastEthernet0/2 del Switch0

* PC3
	* **IP**: 192.168.3.11
	* **Máscara de subred**: 255.255.255.0
	* **Gateway**: 192.168.3.1
	* **Interfaz a la que se conecta**: FastEthernet0/1 del Switch2

* PC4
	* **IP**: 192.168.3.12
	* **Máscara de subred**: 255.255.255.0
	* **Gateway**: 192.168.3.1
	* **Interfaz a la que se conecta**: FastEthernet0/2 del Switch2

* PC5
	* **IP**: 200.10.0.3
	* **Máscara de subred**: 255.255.255.0
	* **Gateway**: 200.10.0.1
	* **Interfaz a la que se conecta**: FastEthernet0/1 del Switch0

* PC6
	* **IP**: 200.10.0.4
	* **Máscara de subred**: 255.255.255.0
	* **Gateway**: 200.10.0.2
	* **Interfaz a la que se conecta**: FastEthernet0/3 del Switch2

* PC7
	* **IP**: 192.168.1.2
	* **Máscara de subred**: 255.255.255.0
	* **Gateway**: 192.168.1.1
	* **Interfaz a la que se conecta**: FastEthernet0/2 del Switch0

* Switch0
	* **Interfaz a la que se conecta**: GigaBitEthernet0/0 del Router1

* Switch1
	* **Interfaz a la que se conecta**: GigaBitEthernet0/0 del Router2

* Switch2
	* **Interfaz a la que se conecta**: GigaBitEthernet0/0 del Router3

* Router1
	* **IP**: 192.168.1.1 /24
	* **Máscara de subred**: 255.255.255.0
	* **Interfaz**: GigaBitEthernet0/0
	* **Interfaz**: Serial0/1/0 **IP**: 10.0.0.1 /30 **Máscara subred**: 255.255.255.252
	* **Interfaz**: Serial0/1/1 **IP**: 10.0.0.5 /30 **Máscara subred**: 255.255.255.252

* Router2
	* **IP**: 192.168.2.1 /24
	* **Máscara de subred**: 255.255.255.0
	* **Interfaz**: GigaBitEthernet0/0
	* **Interfaz**: Serial0/1/0 **IP**: 10.0.0.2 /30 **Máscara subred**: 255.255.255.252
	* **Interfaz**: Serial0/1/1 **IP**: 10.0.0.9 /30 **Máscara subred**: 255.255.255.252

* Router3
	* **IP**: 192.168.3.1 /24
	* **Máscara de subred**: 255.255.255.0
	* **Interfaz**: GigaBitEthernet0/0
	* **Interfaz**: Serial0/1/0 **IP**: 10.0.0.6 /30 **Máscara subred**: 255.255.255.252
	* **Interfaz**: Serial0/1/1 **IP**: 10.0.0.10 /30 **Máscara subred**: 255.255.255.252

## Página 2
Duration: 00:02:00

### Configuración OSPFv2 a los diferentes Routers del esquema.

Positive
: Una vez configurada las IP's de los diferentes host's y routers y conectados por las interfaces correspondientes a los diferentes dispositivos.
Procederemos a configurar ***OSPFv2*** con única **área 0** o *"backbone"*.

Negative
: **Configuración Router1**

![Foto1_PACKET2](img/foto1.jpg)

Le indicamos un ID, y elegimos las redes a las que está conectado directamente el ***Router1*** y indicando el área **"area 0"** o *backbone*.

Positive
: Terminamos con *do wr*, para guardar la configuración y *end*. Para terminar.

![Foto1_2_PACKET2](img/foto1_2.jpg)

Negative
: **Configuración Router2**

![Foto2_PACKET2](img/foto2.jpg)

Aquí lo mismo, indicamos el ID con el que trabajamos del protocolo ***OSPFv2***. Indicamos las redes a las que está conectado directamente el ***Router2*** y indicando el área **"area 0"** o backbone. Y guardamos configuración y finalizamos con *end*.

Negative
: **Configuración Router3**

![Foto3_PACKET2](img/foto3.jpg)

De nuevo, indicamos ID. Y las redes que se conecta directamente el *Router3* y indicando el área **"area 0"** o *backbone*. Guardamos y finalizamos.

## Página 3
Duration: 00:01:30

### Configuración de "Interfaces Pasivas".
### Declararemos una serie de interfaces como pasivas, para que los distintos paquetes que se comunican en el protocolo OSPFv2. No vayan más allá de lo necesario. Es una medida de seguridad. Por si se interceptará tráfico para obtener información de como está estructurada la red. Y dejando que si circulen por las interfaces seriales de los 3 Routers.

Negative
: **Interfaces Pasivas Router1**

![Foto1_3_PACKET2](img/foto1_3.jpg)

Negative
: Lo configuraremos usando de nuevo: ***router ospf 10***.

Negative
: **Interfaces Pasivas Router2**

![Foto2_1PACKET2](img/foto2_1.jpg)

Negative
: **Interfaces Pasivas Router3**

![Foto3_1PACKET2](img/foto3_1.jpg)

## Página 4
Duration: 00:02:30

### Añadimos a OSPFv2, en los dos Routers correspondientes la red, donde se encuentran los equipos de la *VLAN administradores*.

Negative
: **Configuración Router1 Red VLAN administradores**

![Foto1_4_PACKET2](img/foto1_4.jpg)

Negative
: **Configuración Router3 Red VLAN administradores**

![Foto3_4_PACKET2](img/foto3_2.jpg)

Positive
: Indicando como hemos dicho antes. El área principal **area 0**.

## Página 5
Duration: 00:01:00

### Lista de todas las redes con sus adyacencias. En cada uno de los Routers.

Negative
: **Lista de Redes con las adyacencias Router1**

![Foto1_5_PACKET2](img/foto1_5.jpg)

Positive
: Podemos ver con la **"C"**, aquellas que están conectadas directamente con el Router. Y con la **"O"**, aquellas con la que ha hecho adyacencia a través de los Routers vecinos.

Negative
: **Lista de Redes con las adyacencias Router2**

![Foto2_2_PACKET2](img/foto2_2.jpg)

Negative
: **Lista de Redes con las adyacencias Router3**

![Foto3_3_PACKET2](img/foto3_3.jpg)

## Página 6
Duration: 00:04:00

### Lo siguiente, que harémos será configurar la autenticación de OSPFv2 usando el cifrado MD5. De está manera los paquetes no irán en *"texto plano"*.

Negative
: **Configuración autenticación MD5 Router1**

![Foto1_6_PACKET2](img/foto1_6.jpg)

Hemos configurado la interfaz *GigabitEthernet 0/0*, es la que conecta el Router con el Switch de una de las redes.

![Foto1_7_PACKET2](img/foto1_7.jpg)

Aquí, hemos configurado la sub-interfaz, que se crea para la VLAN, ya que en esta red tenemos un host que pertenece a una VLAN de administradores.
Es la *GigabitEthernet 0/0.10*, que conecta el Router con el host de la VLAN.

![Foto1_8_PACKET2](img/foto1_8.jpg)
![Foto1_9_PACKET2](img/foto1_9.jpg)

En este caso pasamos a configurar la interfaz *Serial0/1/0* y *Serial0/1/1* que conecta el Router1 con los otros 2 routers.

Positive
: En el comando como vemos, indicamos primero la interfaz con la que trabajar. Luego le estamos indicando que autenticar con *MD5* e indicando al final la clave. Y terminamos activando la autenticación.



Negative
: **Configuración autenticación MD5 Router2**

![Foto2_3_PACKET2](img/foto2_3.jpg)

Hemos configurado la interfaz *GigabitEthernet 0/0*, es la que conecta el Router con el Switch de una de las redes.

![Foto2_4_PACKET2](img/foto2_4.jpg)
![Foto2_5_PACKET2](img/foto2_5.jpg)

En este caso pasamos a configurar la interfaz *Serial0/1/0* y *Serial0/1/1* que conecta el Router2 con los otros 2 routers.


Negative
: **Configuración autenticación MD5 Router3**

![Foto3_4_PACKET2](img/foto3_4.jpg)

Hemos configurado la interfaz *GigabitEthernet 0/0*, es la que conecta el Router con el Switch de una de las redes.

![Foto3_5_PACKET2](img/foto3_5.jpg)

Aquí, hemos configurado la sub-interfaz, que se crea para la VLAN, ya que en esta red también tenemos el otro host que pertenece a una VLAN de administradores.
Es la *GigabitEthernet 0/0.10*, que conecta el Router con el host de la VLAN.

![Foto3_5_PACKET2](img/foto3_6.jpg)

En este caso pasamos a configurar la interfaz *Serial0/1/0*. Puse solo de una de las interfaces serial. Porque el procedimiento es el mismo. Y creo que se ha entendido.

## Página 7
Duration: 00:00:50

### Demostración que se está usando la autenticación vía MD5 en las interfaces

Negative
: He puesto 2 ejemplos en lo que debemos fijarnos para ver que efectivamente se está usando OSPFv2 con autenticación por MD5.

![Foto1_10_PACKET2](img/foto1_10.jpg)

![Foto3_7_PACKET2](img/foto3_7.jpg)

Positive
: El comando sería: **show ip ospf interface nombre_interfaz**
Y debemos fijarnos en el mensaje de abajo y que se está usando la Key con la ID que creamos.

