PPPoE
#####
:date: 2013-07-14 2:30
:author: Francisco Valladolid
:tags: pppoe, redes, nat, firewall, ipf, ipnat
:category: redes
:slug: pppoe
:lang: es

Introducccion:
--------------

Algunas ocasiones necesitamos establecer una conexion PPPoE (Point to Point over
Ethernet)  a internet via ADSL, los routers actuales
pueden hacer pppoe_ de forma sencilla, sin embargo en algunos entornos donde 
se necesita control de la conexion podemos usar nuestro router casero en modo bridge
y usar NetBSD_ como pppoe/router/firewall/dns cache y tener el control de nuestra red
casera.

Requerimientos:
---------------

    * Una PC corriendo NetBSD_ (pppoe_ es soportado desde 1.6)
    * Dos tarjetas de red (una para el bridge, otra para la red interna)
    * Un editor de textos

Configuracion:
--------------

Suponiendo fxp0 la interfase que conecta al modem ADSL, editamos
*/etc/ifconfig.ppoe* sino existe lo creamos y agregamos lo siguiente:

::

 create
 ! /sbin/ifconfig fxp0 up
 ! /sbin/pppoectl -e fxp0 $int
 ! /sbin/pppoectl $int myauthproto=pap 'myauthname=tu_nombre_de_usuario' \ 
      'myauthsecret=tu_password' hisauthproto=none
        inet 0.0.0.0 0.0.0.1 netmask 0xffffffff
 #! /sbin/route add default -iface 0.0.0.1
 up

**Agregamos ifwatchd al rc.conf para monitorear la conexion.**

``# echo "ifwatchd = YES" >> /etc/rc.conf``

Es necesario bajar el valor del MTU para las interfaces, un valor MTU
alto quiza cause algunos problemas a nuestra conexion, entonces
modificamos dentro de */etc/sysctl.conf*:

`` net.inet.tcp.mss_ifmtu=1``

Si quiere hacer NAT a su red interna, entonces, necesita agregar lo
siguiente a su archivo *rc.conf*

::

 # sysctl -w net.inet.ip.forwarding=1 
 # echo "ipf=YES" >> /etc/rc.conf 
 # echo "ipnat=YES" >> /etc/rc.conf


Haciendo NAT
------------

Agregar las reglas nat a ipnat.conf, suponiendo que esta usando
192.168.1.0/24

::

 map pppoe0 192.168.1.0/24 -> 0/32 portmap tcp/udp 44000:49999 mssclamp 1440
 map pppoe0 192.168.1.0/24 -> 0/32 mssclamp 1440

Con esta configuracion NetBSD_ esta actuando como servidor pppoe_ y haciendo NAT a la red inerna
via ipnat_ e ipf_

Consideraciones:
----------------

NetBSD_ ahora incluye un filtro de paquetes en base llamado npf_ para hacer NAT y 
filtrado de paquetes, la configuracion para ello varia en mucho en comparacion a ipf_.

.. _NetBSD: http://www.netbsd.org
.. _pppoe: http://netbsd.gw.com/cgi-bin/man-cgi?pppoe++NetBSD-current
.. _ipnat: http://netbsd.gw.com/cgi-bin/man-cgi?ipnat++NetBSD-current
.. _ipf: http://netbsd.gw.com/cgi-bin/man-cgi?ipf++NetBSD-current
.. _npf: http://netbsd.gw.com/cgi-bin/man-cgi?npf++NetBSD-current


