Usando pkgsrc e instalando paquetes binarios
############################################
:date: 2013-06-26 16:40
:tags: pkgsrc, binarios, pkgin, paquetes
:category: NetBSD
:author: Francisco Valladolid
:slug: pkgsrc-binarios
:lang: es

¿Que es pkgsrc ?
================

.. image:: _static/pkgsrc.png

pkgsrc_ es un con junto de makefiles que permiten la creacion e
instalacion de software de terceros en NetBSD_ y otros sistemas tipo
UNIX, actualmente pkgsrc_ cuenta con mas de 12000 paquetes y es
independiente del sistema base, lo que lo hace altamente portable y
eficiente. A continuacion se detalla su uso.

Inicializando pkgsrc
====================

Establecemos las variables usadas por cvs
-----------------------------------------

::

  # export CVSROOT=:pserver:anoncvs@anoncvs.netbsd.org:/cvsroot
  # export CVS_RSH=ssh

Descargamos pkgsrc a traves de CVS por unica vez
------------------------------------------------

::

  # cd /usr
  # cvs -d$CVSROOT login
  # cvs -d$CVSROOT co -P pkgsrc
  # cvs -d$CVSROOT logout

Con estas simples instrucciones tenemos instalado y actualizado el arbol
de paquetes de NetBSD.

Instalando paquetes via pkgsrc
==============================

Para instalar ImageMagick desde pkgsrc:
---------------------------------------

::

  # cd /usr/pkgsrc/graphics/ImageMagick
  # make install clean

Para crear un paquete precompilado de ImageMagick desde pkgsrc:
---------------------------------------------------------------

::

  # cd /usr/pkgsrc/graphics/ImageMagick
  # make package

el paquete se creara en /usr/pkgsrc/packages/All/ImageMagick-VERSION.tgz

Para checar la lista de paquetes que hemos instalado:

``# pkg_info``

Para desinstalar un paquete previamente instalado, (averigue el nombre
correcto, este nombre del paquete es de ejemplo).

``# pkg_delete ImageMagick-VERSION.tgz``

Introduccion a pkgin
====================

NetBSD_ ofrece una utileria llamada ``pkgin`` al estilo ``pacman`` en
ArchLinux_ o ``yum`` en Fedora_, que permite instalar paquetes binarios
precompilados para una plataforma especifica, veamos como trabaja.

Si acabas de instalar NetBSD y no quieres lidiar compilando paquetes via
pkgsrc_, asumimos NetBSD_ 6.1 amd64 para este articulo.

El instalador de NetBSD_ a partir de 6.0, te permite instalar ``pkgin``
automaticamente, si lo omitiste, entonces ve al sitio ftp
``ftp://ftp.netbsd.org/pub/pkgsrc/packages/NetBSD/amd64/6.1/All/`` y
descarga ``pkgin-0.6.3.1.1.tgz`` e instalalo manualmente via pkg_add

::

  # pkg_add -v pkgin-0.6.3.1.1.tgz

``pkgin`` usa un archivo de configuracion en
*/usr/pkg/etc/pkgin/repositories.conf* el  cual necesitas editar 
para que apunte a tu ftp actual de paquetes binarios, en nuestro caso;
``ftp://ftp.netbsd.org/pub/pkgsrc/packages/NetBSD/amd64/6.1/All/``

Siguiente paso, actualizar el arbol de paquetes localmente;

``# pkgin update``

Usando pkgin
------------
Ejecutando pkgin sin argumentos, obtenemos:
  ::

    $ pkgin
      Usage: pkgin [-cdfFhlnPtvVy] command [package ...]
      Commands and shortcuts:
      list                (ls  ) -  List installed packages.
      avail               (av  ) -  List available packages.
      install             (in  ) -  Perform packages installation or upgrade.
      update              (up  ) -  Create and populate the initial database.
      remove              (rm  ) -  Remove packages and depending packages.
      upgrade             (ug  ) -  Upgrade main packages to their newer
      versions.
      full-upgrade        (fug ) -  Upgrade all packages to their newer
      versions.
      show-deps           (sd  ) -  Display direct dependencies.
      show-full-deps      (sfd ) -  Display dependencies recursively.
      show-rev-deps       (srd ) -  Display reverse dependencies recursively.
      show-category       (sc  ) -  Show packages belonging to category.
      show-pkg-category   (spc ) -  Show package's category.
      show-all-categories (sac ) -  Show all categories.
      keep                (ke  ) -  Mark package as "non auto-removable".
      unkeep              (uk  ) -  Mark package as "auto-removable".
      show-keep           (sk  ) -  Display "non auto-removable" packages.
      show-no-keep        (snk ) -  Display "auto-removable" packages.
      search              (se  ) -  Search for a package.
      clean               (cl  ) -  Clean packages cache.
      autoremove          (ar  ) -  Autoremove orphan dependencies.
      export              (ex  ) -  Export "non auto-removable" packages to
      stdout.
      import              (im  ) -  Import "non auto-removable" package list
      from file.
      provides            (prov) -  Show what files a package provides.
      requires            (req ) -  Show what files a package requires.
      pkg-content         (pc  ) -  Show remote package's content.
      pkg-descr           (pd  ) -  Show remote package's long-description.
      pkg-build-defs      (pbd ) -  Show remote package's build definitions.


Ejemplos:
---------
::

  # pkgin se sudo             ;Busca sudo en el arbol local de paquetes

  # pkgin install sudo        ;Instala sudo y sus dependencias

  # pkgin remove sudo         ;quita  sudo del sistema

  # pkgin list                ;Muestra los paquetes instalados

  # pkgin                     ;Ayuda


.. _NetBSD:    http://www.netbsd.org
.. _ArchLinux: http://www.archlinux.org
.. _Fedora:    http://www.fedoraproject.org
.. _pkgsrc:    http://www.pkgsrc.org

