Instalación de NetBSD con Arranque desde Red (PXE)
##################################################
:date: 2012-11-02 09:28
:tags: Redes, PXE, DHCP, TFTP
:category: NetBSD
:author: César Yáñez Fernández
:slug: instalacion-red
:lang: es

NetBSD_ es un Sistema Operativo bastante flexible; en algunos escenarios, tenemos la necesidad de instalar el Sistema Operativo desde red, dentro de un equipo destino que no cuenta con disquetera ni puede arrancar por USB. El CD-ROM de NetBSD_ cuenta con los archivos necesarios para poder instalarse desde red. En éste artículo explicaremos lo necesario para un arranque usando PXE_ (Preboot eXecution Environment); el cual es muy usado en la actualidad.

Servidor de Arranque
====================
El servidor de arranque necesita tener activado los servicios de DHCP_, TFTP_ y de manera opcional el servicio de HTTP o FTP donde contendrá los componentes para su instalación.

Servicio de DHCP
----------------
En el servicio de DHCP_, se deberá de dar de alta una dirección IP fija que será asignada al equipo destino y se indicará la dirección IP del Servidor que tenga el servicio de TFTP en la variable ``next-server``. Si se usa el servidor DHCP_ de ISC la configuración sería similar a la que sigue:

::

  host MiServidor {
    hardware ethernet 00:AA:11:BB:22:CC;
    fixed-address 192.168.1.105;
    filename "pxeboot_ia32.bin";
    next-server 192.168.1.101;
    option host-name "nbsd-server";
    option routers 192.168.1.254;
  }

Servicio de TFTP
----------------
Para el servidor TFTP_ se puede usar la opción que les sea más cómoda, y no requiere de una configuración especial, lo importante aqui es obtener los siguientes archivos del CD-ROM de NetBSD_ y colocarlos en el directorio raíz que provee el servicio de TFTP_:

- ``amd64/installation/misc/pxeboot_ia32.bin``
- ``amd64/binary/kernel/netbsd-INSTALL.gz``

En el caso del kernel de instalación llamado ``netbsd-INSTALL.gz``; hay que descomprimirlo usando ``gunzip`` o  ``zcat`` y nombrarlo como ``netbsd``, aunque el nombre de ``netbsd-INSTALL`` puede quedarse si gustan.

Servicio de HTTP o FTP
----------------------
Pueden usar el servidor de su elección y éste servicio es opcional, ya que el asistente de instalación de NetBSD_ preguntará en donde se encuentran los sets de instalación. Para usarlo es colocar en el directorio que provee los archivos del servicio, todo el árbol del CD-ROM de NetBSD_ en el mismo orden.

Equipo Destino
==============
El equipo destino debe de tener en el ROM de su tarjeta de red el soporte para arranque por PXE_, ésto puede verse en la interfaz de la ROM de la terjeta de red o en el Firmware (BIOS) del equipo. Al equipo le indicaremos que el dispositivo de arranque será la tarjeta de red. Una vez que obtenga los datos del servidor DHCP_, le indicamos que el kernel de NetBSD_ se encuentra en TFTP_, tecleando ``boot tftp:netbsd``:

::

  CLIENT MAC ADDR: 00 AA 11 BB 22 CC  GUID: 564DAF7F-7E8A.0CDE-A2A6-FB54233D2014
  CLIENT IP: 192.168.1.105  MASK: 255.255.255.0  DHCP IP: 192.168.1.101
  GATEWAY IP: 192.168.1.254;
  
  >> NetBSD/x86 PXE boot, Revision 5.1 (from NetBSD 6.0)
  >> Memory: 572/260032 k
  Press return to boot now, any other key for boot menu
  booting netbsd - starting in 0 seconds.
  type "?" or "help" for help.
  > boot tftp:netbsd

Descargará el kernel del servidor TFTP_, y una vez arrancado llegará al asistente de instalación.

Esperemos que éste pequeño artículo les sea de utilidad.

.. _NetBSD: http://www.netbsd.org
.. _PXE: http://download.intel.com/design/archives/wfm/downloads/pxespec.pdf
.. _DHCP: http://www.isc.org/sw/dhcp
.. _TFTP: https://tools.ietf.org/html/rfc1350
