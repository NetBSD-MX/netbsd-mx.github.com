Instalando modular-xorg desde pkgsrc
====================================

Recientemente tuve la necesidad de configurar *X* en mi *IBM Thinpad
T43p*, la cual incluye una tarjeta gráfica *ATI Fire Gl* con 128MB.

Descargué `NetBSD`_ **-current**\ (4.99.45) desde ftp y proseguí a
realizar unn a instalación completa, no fue hasta que traté de
configurar X para poder usarr mi *laptop* como *desktop* para hacer
tareas comunes, y no conseguí hacer funcc ionar X correctamente,
`XFree86`_ simplemente no trabajó para mí.

Antes de empezar
----------------
No instalar `XFree86`_ que viene en base, de-seleccionar en el programa
de instalación.

Instalando modular-xorg desde *pkgsrc*
------------------------------------------

Estoy usando `pkgsrc`_ **-current** para aprovechar algunas novedades,
las instrucciones para compilar ``modular-xorg`` desde `pkgsrc`_ son las
siguientes:

-  Editar ``/etc/mk.conf`` e incluir: ``X11_TYPE=modular``

-  Los paquetes necesarios son los siguientes:

``# cd /usr/pkgsrc/x11/modular-xorg-server``

``# make install clean``

``# cd /usr/pkgsrc/meta-pkgs/modular-xorg-apps``

``# make install clean``

``# cd /usr/pkgsrc/meta-pkgs/modular-xorg-fonts``

``# make install clean``

``# cd /usr/pkgsrc/x11/xf86-input-keyboard``

``# make install clean``

``# cd /usr/pkgsrc/x11/xf86-input-mouse``

``# make install clean``

-  Esta es la parte interesante, elegir el controlador de video (*video
   driver*) que estés usando en tu computadora o *laptop*, en mi caso
   estoy usando ATI, parr a ello:

``# cd /usr/pkgsrc/x11/xf86-video-ati``

``# make install clean``

Configurar *Xorg*
-----------------

Si todo va bien, proseguimos a configurar `Xorg`_ de dos formas.

``# xorgcfg``

El comando anterior inicia el programa de configuración de `Xorg`_ en
modo grr áfico y el siguiente en modo texto.

``# xorgconfig``

Configurando lo básico en .xinitrc
----------------------------------------

``xclock -geometry 50x50-1-1 &``

``xterm -geometry 80x34-1+1 -bg OldLace &``

``xsetroot -solid Bisque4 &``

``xterm -geometry 80x44+0+0 -bg AntiqueWhite -name login``

``twm``

Iniciando *Xorg*
----------------

``# xinit``

Referencias
-----------

.. _NetBSD: http://www.netbsd.org/
.. _XFree86: http://www.xfree86.org/
.. _pkgsrc: http://www.pkgsrc.org/
.. _Xorg: http://www.x.org/
