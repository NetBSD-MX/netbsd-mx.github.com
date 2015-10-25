pkgsrc en MacOSX
################

.. contents::

Introduccion
-------------

pkgsrc_ es un sistema de paquetes altamente portable, originalmennte creado para el sistema operativo NetBSD_ 
pero puede usarse ahora en una variad de sistemas, incluyendo Linux, Minix, FreeBSD y Mac OSX.

En este articulo, detallaremos los pasos para usar paquetes binarios precompilados en un sistema OSX.

Requerimientos
--------------
* OS X 64 bit
* culr
* iTerm.app o Terminal.app

Pasos
-----
::

    : Descargamos el kit bootstrap de 64-bit
    $ curl -Os http://pkgsrc.joyent.com/packages/Darwin/bootstrap/bootstrap-2015Q1-x86_64.tar.gz
    
    ; Verificamos el checksum

    $ shasum bootstrap-2015Q1-x86_64.tar.gz
    2d2f8dda3743dcd323c306828e9dbdc63b09bff2  bootstrap-2015Q1-x86_64.tar.gz
    
    ; Instalamos el bootstrap en el directorio /
    
    $ sudo tar -zxpf bootstrap-2015Q1-x86_64.tar.gz -C /

    ;Agregamos las rutas para los directorios del bootstrap
    
    $ sudo tar -zxpf bootstrap-2015Q1-x86_64.tar.gz -C /
    
    $ PATH=/usr/pkg/sbin:/usr/pkg/bin:$PATH
    $ MANPATH=/usr/pkg/man:$MANPATH

    ; Actualizar la base de datos de paquetes

    $ sudo pkgin -y update
    
    ; total de paquetes disponibles.

    $ pkgin avail | wc -l 

Con estos sencillos pasos, estaras usando pkgsrc en tu sistema operativo OSX y sentirte en casa, instalar
paquetes una vez actualizado la base de datos es muy sencillo, tenemos un tutorial sobre pkgin en este sitio.

Captura de pantalla.
--------------------

.. image:: _static/pkgsrc-osx.png


.. _NetBSD: http://www.netbsd.org
.. _rc: http://netbsd.gw.com/cgi-bin/man-cgi?rc.d++NetBSD-current
.. _nginx: http://www.nginx.org
.. _pkgsrc: http://www.pkgsrc.org
.. _binarios: http://www.netbsd.mx/pkgsrc-binarios.html

