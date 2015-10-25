PostgreSQL 
##########

.. contents::

Introduccion
-------------

PostgreSQL_ es un poderoso Sistema Relacional de Base de Datos basado en Sofware
libre, con soporte para numerosos sistemas tipo Unix y Windows.

Requerimientos
--------------
* NetBSD 6 o superior
* pkgsrc (2014-Q4) o binarios para 6.0 +
* pkgin (si va a usar binarios)

Configuracion
-------------
En esta ocasion voy a usar pkgsrc-2014Q4 para instalar postgresql-server.
Usando pkgsrc en lugar de binarios asegura tener la ultima version de software.
Actualizamos nuestro arbol de paquetes andes de proceder.

::

    $ cd /usr/pkgsrc/
    $ sudo cvs -q up -dP
    $ cd /usr/pkgsrc/databases/postgresql93
    $ sudo make install clean

Es importante agregar las lineas siguientes al archivo */etc/rc.conf* para iniciar
los servicios automaticamente.

::

    pgsql=YES
    pgsql_flags="-l"

pkgsrc_ coloca los scripts de inicio en */usr/pkg/share/examples/rc.d/*, de ahi que tenemos que copiar
*/usr/pkg/share/examples/rc.d/pgsql a /etc/rc.d/*

Iniciando PostgreSQL
--------------------

::

    $ sudo /etc/rc.d/pgsql start

Para hacer labores iniciales de administracion, quizas inicies el comando *psql* con el usuario pgsql
y la base de datos postgres

::

    $ psql -U pgsql postgres
    psql (9.3.5)
    Type "help" for help.

    postgres=#

Autenticacion
-------------

El archivo */usr/pkg/pgsql/data/pg_hba.conf* contiene la configuracion para establecer la conexion
al motor de base de datos y es el archivo de configuracion del cliente, el cual nos permite elegir
un metodo de autenticacion y permitir o denegar la conexion remota a nuestro sistema de base de datos.

Mas informacion puede encontrarse en el sitio oficial de PostgreSQL_


.. _NetBSD: http://www.netbsd.org
.. _rc: http://netbsd.gw.com/cgi-bin/man-cgi?rc.d++NetBSD-current
.. _nginx: http://www.nginx.org
.. _pkgsrc: http://www.pkgsrc.org
.. _binarios: http://www.netbsd.mx/pkgsrc-binarios.html
.. _PostgreSQL: http://www.postgresql.org

