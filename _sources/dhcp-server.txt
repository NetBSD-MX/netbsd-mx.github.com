Servidor DHCP
#############

Configurando un servidor DHCP es simple. NetBSD incluye `ISC
DHCP <http://www.isc.org/sw/dhcp/>`__ en el sistema base, asi que no es
necesario instalar software adicional.

La configuracion de **dhcpd** radica en el archivo */etc/dhcpd.conf*, la
forma basica:

::

 deny unknown-clients;       
 ddns-update-style none;       
 subnet 192.168.1.0 netmask 255.255.255.0 {
        range 192.168.1.50 192.168.1.100; 
        default-lease-time 28800;
        max-lease-time 86400;
        option broadcast-address 192.168.1.255;
        option domain-name "mi-dominio.com";
        option domain-name-servers 200.33.146.241, 200.33.146.249;
        option routers 192.168.1.1;
        host www-server {
                hardware ethernet 00:00:00:00:00:00;
                fixed-address 192.168.1.3;
                }
 }



En la configuracion anterior, tenemos direcciones IP asignadas pr DHCP
desde 192.168.1.50 hasta 192.168.1.100, ademas un gateway en 192.168.1.1
y una direccion fija para el host www-server 192.168.1.3 identificada
por su MAC (00:00 es supuesto)

Editamos */etc/rc.conf* para activar **dhcpd** al momento del arranque,
asi como *dhcpd\_flags*

::

 dhcpd=YES
 dhcpd_flags="-q fxp0"


*dhcpd\_flags* pasa el parametro al servidor **dhcpd** para definir la
interfaz que escuchara las solicitudes de IP, en nuestro caso es una
intel 10/100 (fxp0)

Antes de iniciar el servidor dhcpd, es necesario crear el archivo
*/var/db/dhcpd.leases*

::

 # touch /var/db/dhcpd.leases


Iniciamos el servidor:

::

 # /etc/rc.d/dhcpd start

Si todo va bien, en este momento NetBSD esta actuando como servidor DHCP
a la red interna.


Referencias:
------------

-  `man dhcpd <http://netbsd.gw.com/cgi-bin/man-cgi?dhcpd>`__

-  `HOWTO: DHCP Server <http://www.netbsd.org/docs/network/dhcp.html>`__

