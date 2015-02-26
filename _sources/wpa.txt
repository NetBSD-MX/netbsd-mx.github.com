WPA
###

Requisitos
----------

Se requiere NetBSD 4 o superior.

Que es ?
--------

WPA_ del ingles
WiFi Protected Access, es un medio usado para proteger redes
inalambricas. NetBSD soporta WPA a traves de
wpa_supplicant_  que esta incluido en base.

Configuracion
-------------

Para conectarnos a un AP usando WPA-Personal necesitamos wpa_supplicant_\(8) que contenga al menos:

::

 /etc/wpa_supplicant.conf

 network = {
     ssid="MYWLAN"
     scan_ssid=1
     key_mgmt=WPA-PSK
     psk="Mi_llave_psk"
 }

Para crear llaves podemos usar wpa_passphrase_\(8) 

::

 # wpa_passphrase myap alguna_clave
 
 network={
   ssid="myapp"
   psk=39db2863271f7a2ba6549e0941c95154ff339982a7849c71082561eb570e6708
 }

Por citar un ejemplo, es importante notar que psk= debe ir sin comillas
ni apostrofes.

Siempre podremos agregar mas redes a nuestro archivo de configuración
wpa\_supplicant.conf, ademas wpa\_supplicant soporta muchos protocolos
(WPA, WPA2,IEEE 802.1x) y varios métodos para autentificar (EAP,
PEAP,etc), podremos tener también una red "genérica" para redes abiertas
al publico (En algunos restaurantes, hoteles y aeropuertos) que puede
describirse como sigue:

::
    
 network={
     ssid="any"
     key_mgmt=NONE
     priority=2
 }
 

Al final solo debemos solicitar una IP y navegaremos sin problemas.


Usando wpa\_supplicant
----------------------

Agrega lo siguiente a tu */etc/ifconfig.ral0*:

::

  up
  dhcp

y mediante las siguientes lineas en /etc/rc.conf, inmediatamente despues
de *wpa\_supplicant=YES*

::

 dhcpcd_flags="-q -b" 
 wpa_supplicant=YES
 wpa_supplicant_flags="-B -i ral0 -c /etc/wpa_supplicant.conf"

La opcion -B permite correr wpa\_supplicant en modo background.

Mas informacion:
----------------
La pagina del manual de wpa_supplicant.conf_ contiene toda la informacion del proceso mencionado en este tutorial.


.. _wpa_passphrase: http://netbsd.gw.com/cgi-bin/man-cgi?wpa_passphrase++NetBSD-current
.. _wpa_supplicant: http://netbsd.gw.com/cgi-bin/man-cgi?wpa_supplicant+8+NetBSD-current
.. _wpa_supplicant.conf: http://netbsd.gw.com/cgi-bin/man-cgi?wpa_supplicant.conf+5+NetBSD-current
.. _WPA: http://es.wikipedia.org/wiki/Wi-Fi_Protected_Access
