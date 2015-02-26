NEMP - NetBSD - nginx - PHP 
###########################

.. contents::

Introduccion
-------------

NetBSD_ es un sistema operativo completo y puede usarse para ejecutar aplicaciones en servidores web. Apache fue por mucho 
tiempo el servidor web favorito y ampliamente soportado.
Con la introduccion de servicios web dinamicos y stream, asi como las redes sociales con un amplio flujo de datos, se repeenso
la idea de un servidor web moderno, ligero y con soporte para las necesidades actuales, aqui entra nginx_

Requerimientos
--------------
* NetBSD 6 o superior
* pkgsrc (2014-Q4) o binarios para 6.0 +
* pkgin (si va a usar binarios)
* Muchas ganas.

Configuracion
-------------

Primeramente debemos instalar nginx_ desde pkgsrc_ o binarios, usaremos binarios_ por comodidad.

::

  $ sudo pkgin install nginx

Posteriormente, instalar *php-fpm*

::

  $ sudo pkgin install php53-fpm

Editamos la seccion basica de */usr/pkg/etc/nginx.conf* para agregar soporte PHP a nginx_, la directiva location
se coloca dentro de la seccion *server* del archivo de configuracion.
 
.. code::

  location ~ \.php$ {
    root         /var/www/site/html;
    # for a TCP stream
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  /var/www/site/html$fastcgi_script_name;
    include        /usr/pkg/etc/nginx/fastcgi_params;
  }

El archivo */usr/pkg/etc/php-fpm.conf* debe contener al menos lo siguiente:

::

  ; Unix user/group of processes
  ; Note: The user is mandatory. If the group is not set, the default user's group
  ;       will be used.
  user = nginx
  group = nginx

  ; The address on which to accept FastCGI requests.
  ; Valid syntaxes are:
  ;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific address on
  ;                            a specific port;
  ;   'port'                 - to listen on a TCP socket to all addresses on a
  ;                            specific port;
  ;   '/path/to/unix/socket' - to listen on a unix socket.
  ; Note: This value is mandatory.
  listen = 127.0.0.1:9000
  

Es importante agregar las lineas siguientes al archivo */etc/rc.conf* para iniciar los servicios automaticamente.

::

  nginx=YES
  php-fpm=YES

pkgsrc_ coloca los scripts de inicio en */usr/pkg/share/examples/rc.d/*, es tarea sencilla copiar los scripts 
al directorio /etc/rc.d para mayor comodidad e iniciar nginx_ y php-fpm.

::

  $ sudo /etc/rc.d/nginx start
  $ sudo /etc/rc.d/php-fpm start

Pruebe el servidor editando un simple archivo php y colocandolo en la ruta publica de su servidor, en nuestro caso */var/www/site/html*

.. code::
  
  <?php
     phpinfo();
  ?>

Es todo.

.. _NetBSD: http://www.netbsd.org
.. _rc: http://netbsd.gw.com/cgi-bin/man-cgi?rc.d++NetBSD-current
.. _nginx: http://www.nginx.org
.. _pkgsrc: http://www.pkgsrc.org
.. _binarios: http://www.netbsd.mx/pkgsrc-binarios.html

