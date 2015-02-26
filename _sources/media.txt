Grabando CD's and DVD-R
#######################

Instalando lo necesario
-----------------------

-  (DVD+RW-TOOLS) - pkgsrc/sysutils/dvd+rw-tools
-  (CDR-DAO) - pkgsrc/sysutils/cdrdao
-  (CDR-TOOLS) - pkgsrc/sysutils/cdrtools-ossdvd

Grabando DVD's
--------------

``# growisofs  -dvd-compat -Z /dev/rcd0d=imagen.iso``

Almacenando archivos en un DVD
------------------------------

``# growisofs -dvd-compat -Z /dev/rcd0d -J -R /path/to/files``

Creando un DVD Video
--------------------

Para crear un DVD-Video desde una ruta especifica conteniendo VIDEO\_TS
y AUDIO\_TS hacemos:

``# growisofs -Z /dev/rcd0d -dvd-video /path/to/video/files``

Generando una imagen desde un DVD
---------------------------------

``# readcd dev=/dev/rcd0d f=image.img``

Formateando DVD's
-----------------

``# dvd+rw-format -blank /dev/rcd0d``

Generando una imagen .ISO desde un CD
-------------------------------------

``#  readcd dev=/dev/rcd0d  f=imagen.iso``

Grabando una imagen ISO a un CD
-------------------------------

| ``# cdrecord -v dev=/dev/rcd0d imagen.iso``

Formateando CD's
----------------

``# cdrdao blank --device /dev/rcd0d --driver generic-mmc``

Trabajando con CD de Audio
--------------------------

El comando siguiente lee las pistas de audio en formato WAV y genera una
tabla de contenidos en el archivo cdtracks.toc 

``# cdrdao read-cd --device /dev/rcd0d cdtracks.toc``

A continuacion escribimos el contenido del CD de audio originado a
partir de la tabla de contenidos del paso anterior en un CD-R blanco. #
cdrdao write --device /dev/rcd0d cdtracks.toc

Como puedes leer, NetBSD incluye todo lo necesario para generar tus CD's
y DVD's desde el shell, tu eres libre de programar tus scripts y
automatizar tus copias.

Links:

- `NetBSD-creando boot  disks <http://www.netbsd.org/docs/bootcd.html>`__
-  `dvd+rw-tools <http://fy.chalmers.se/~appro/linux/DVD+RW/>`__
-  `cdrdao <http://cdrdao.sourceforge.net/>`__

