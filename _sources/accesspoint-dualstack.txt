Access point con doble stack y seguridad WPA
############################################
:date: 2013-07-02 16:40
:tags: netbsd,redes, wireless
:category: NetBSD
:author: Heron Gallegos
:slug: accesspoint-dualstack
:lang: es

.. contents::

Descripcion
-----------

Este Access Point operando bajo NetBSD con llaves dinámicas WPA es
extremadamente seguro. Las razones de ello no podría explicarlas porque
esa no es mi área de conocimiento... tendrá qué buscarlas usted por su
cuenta. Si usted es un paranoico de la seguridad, esta configuración de
Access Point es para usted.

Definiciones:
--------------

-  La tarjeta de red es la CNet **CWP-854** PCI con chipset
   RT2501/RT2561N driver **ral(4)**
-  Se utilizará una llave de 512 bits (64 bytes)
-  Se utlizará la red privada 192.168.154.0/24
-  Se utlizará la red IPv6 3ffe:8240:8018:7fa::/64
-  Las direcciones de nuestra tarjeta inalámbrica serán 192.168.154.1 y
   3ffe:8240:8018:7fa::1
-  Nuestra red inalámbrica se llamará **hola**
-  Utilizaremos el **canal 4** de la red inalámbrica

Requisitos:
--------------

-  Una computadora con NetBSD-4 ó superior
-  Servicio dhcpd en la misma máquina configurado con las direcciones
   previamente definidas
-  Servicio DNS caché (por lo menos) en la misma máquina
-  Servicio hostapd
-  Una llave de 512 bits en formato exadecimal generadas en algún
   dispositivo de tipo random

Procedimiento:
--------------

-  Instale el hardware y software
-  Configure el sistema operativo y los requisitos, no olvide el soporte
   de IPv6
-  Elabore el archivo de configuración de la tarjeta inalámbrica

   ``/etc/ifconfig.ral0``

::

  nwid hola mediaopt hostap chan 4
  inet  192.168.64.1 netmask 0xffffff00 media auto
  inet6 3ffe:8240:8018:7fa::1 prefixlen 64 alias
  up

-  Haga el archivo **/etc/rtadvd.conf** de anuncio de rutas y asignación
   de direcciones de IPv6

 ``ral0:\``
 ``        :addr="3ffe:8240:8016:aff::":prefixlen#64:``

-  Haga el archivo **/etc/hostapd.conf** para la configuración de la
   seguridad

::

  interface=ral0
  logger_syslog=-1
  logger_syslog_level=2
  logger_stdout=-1
  logger_stdout_level=2
  debug=0
  dump_file=/tmp/hostapd.dump
  ctrl_interface=/var/run/hostapd
  ctrl_interface_group=wheel
  ssid=hola
  macaddr_acl=0
  auth_algs=1
  wpa=1
  wpa_psk=75eb38988c5bb4499517c319934c845f6575f0456fa693666e0a90da3b92b740

-  Configure el arranque de los servicios en **/etc/rc.conf**

::

 ip6mode=router
 hostapd=YES
 rtadvd=YES      rtadvd_flags="ral0"
 named=YES       named_flags=""
 dhcpd=YES       dhcpd_flags="-q ral0"

Reinicie su máquina, la parte de Access Point ha quedado lista.

Cliente:
--------

-  Configure la interfase inalámbrica del cliente con la red hola y la
   llave de 512 bits (wpa\_psk)
-  Reinicie

Ahora tiene usted el servicio de Access Point WPA completamente
operacional. Disfrute su nuevo access point :)

Comentarios y explicaciones:
----------------------------

 ``/etc/ifconfig.ral0``

-  hostap es el modo para trabajar como "Access Point". Otras
   opciones son ibss para conexiones peer to peer, **monitor**
   (autoexplicativo) y bss (default) para ser cliente de un access
   point. Consulte el manual del driver de su tarjeta de red para
   comprobar el soporte de estos modos.
-  Se hacen las asignaciones de direcciónes IP, IPv6 y sus máscaras. La
   media elegida es auto para que la tarjeta inalámbrica tome el
   valor default de velocidad. Otros valores posibles son DS5,
   OFDM54 y varios más. Consulte el manual del driver de su tarjeta
   de red para comprobar el soporte de otras velocidades así como sus
   alcances y limitaciones.

 ``/etc/rtadvd.conf``

-  Con ral0:addr="3ffe:8240:8018:7fa::":prefixlen#64: es posible
   definirlo en una sola línea acorde a la sintáxis de termcap(5)
-  El servicio rtadvd con la dirección base addr/64 y la mac address del
   cliente que solicita una IPv6 le asignará una dirección utilizando el
   formato eui64

 ``/etc/rc.conf``

-  Ponemos nuestra máquina con NetBSD en modo de ruteo IPv6
-  Declaramos que los cuatro servicios arrancarán en automático. Las
   flags con ral0 indican que el servicio solo correrá en esa interfase.

 ``/etc/hostapd.conf``

-  El archivo proporcionado está a su mínima expresión, con valores por
   default y sin comentarios.
-  Si usted desea tener este archivo completo y con sus comentarios,
   haga lo siguiente:

``cp /usr/share/examples/hostapd/hostapd.conf /etc/hostapd.conf``

Edite su archivo y haga los siguientes reemplazos

::

  diff /usr/share/examples/hostapd/hostapd.conf /etc/hostapd.conf
  1c1
  < #     $NetBSD: hostapd.conf,v 1.1 2006/04/30 13:52:35 rpaulo Exp $
  ---
  > #     $NetBSD: hostapd.conf 1.1 2006/04/30 13:52:35 rpaulo Exp $
  8c8
  < interface=if0
  ---
  > interface=ral0
  70c70
  < ssid=NetBSD
  ---
  > ssid=hola
  90c90
  < auth_algs=3
  ---
  > auth_algs=1
  268c268
  < #wpa=1
  ---
  > wpa=1
  276c276
  < #wpa_psk=0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef
  ---
  >wpa_psk=75eb38988c5bb4499517c319934c845f6575f0456fa693666e0a90da3b92b740

Si usted no quiere IPv6, suprima las líneas que lo relacionan, pero no
lo eche en saco roto. **IPv6 ya es inminente**.

Disfrute su nuevo Access Point con NetBSD

