==============================
20: Logueos con autenticación
==============================

Una vista para ingresar que autentifica un nombre de usuario/contraseña contra una lista de
usuarios.

Fondo
======

La mayoría de las aplicaciones web tienen direcciones URL que permiten a los usuarios añadir/editar/borrar
contenido a través de un navegador web.
Es tiempo de agregar
:ref:`seguridad <security_chapter>` a la aplicación. 
En este primer paso, nos vamos a introducir en la autenticación.
Es decir,  el registro y cierre de sesión de los usuarios conectados
utilizando las instalaciones Pyramid's de forma fácil.


En el siguiente paso nos vamos a introducir en la protección de recursos
con declaraciones de seguridad para la autorización.

Objetivos
==========

- Introducción de los conceptos de autenticación

- Crear vistas para ingresar/salir


Pasos
=====

#. Vamos a utilizar la clase "vista" como punto de partida:

   .. code-block:: bash

    $ cd ..; cp -r view_classes authentication; cd authentication
    $ $VENV/bin/python setup.py develop

#. Colocar el hash de seguridad en el archivo ``authentication/development.ini``
   de configuración como ``tutorial.secret`` en vez de colocarlo directo en
   el código:

   .. literalinclude:: authentication/development.ini
    :language: ini
    :linenos:

#. Obten la autenticación (y por ahora, las políticas de autorización) y la ruta
   de ingreso dentro de :term:`configurator` en
   ``authentication/tutorial/__init__.py``:

   .. literalinclude:: authentication/tutorial/__init__.py
    :linenos:

#. Crear un módulo: ``authentication/tutorial/security.py`` que puede encontrar
    nuestra información del usuario al proporcionar la llamada a la política 
    de autenticación
  
   .. literalinclude:: authentication/tutorial/security.py
    :linenos:

#. Actualizar la vista en: ``authentication/tutorial/views.py``:

   .. literalinclude:: authentication/tutorial/views.py
    :linenos:

#. Agregar una plantilla de ingreso en: ``authentication/tutorial/login.pt``:

   .. literalinclude:: authentication/tutorial/login.pt
    :language: html
    :linenos:

#. Proporcionar un inicio de sesión y salida del sistema en: ``authentication/tutorial/home.pt``

   .. literalinclude:: authentication/tutorial/home.pt
    :language: html
    :linenos:

#. Ejecuta tu aplicación Pyramid con:

   .. code-block:: bash

    $ $VENV/bin/pserve development.ini --reload

#. Abre http://localhost:6543/ en tu navegador.

#. Click en "Inicio de sesión".

#. Envíe el formulario de inicio de sesión con el nombre de usuario y la contraseña
   ``editor``.

#. Notar que el botón "Inicio de sesión" cambió a "Salir".

#. Click en "Salir" link.

Análisis
========

A diferencia de muchos frameworks web, Pyramid incluye un modelo de seguridad 
incorporada pero que es opcional para la autenticación y autorización. Este 
tipo de seguridad en el sistema está destinado a ser flexible y apoyar a 
muchas necesidades. En este modelo de seguridad, la autenticación (quién eres) 
y la autorización (qué tienes permitido hacer) no son conectable, pero se pueden
desvincular.
Pero aprenderemos un paso a la vez, vamos a proveer un sistema con inicio y salida
del mismo.

En éste ejemplo se optó por usar el paquete:
:ref:`AuthTktAuthenticationPolicy <authentication_module>`.
Lo activamos en nuestra configuración y proporcionamos un
"ticket-signing" en nuestro archivo INI.

Nuestra clase de la "vista" nos extendió un inicio de sesión. 
Cuando llegamos a ella, a través de un GET, ella retorna
un formulario de inicio de sesión. Al llegar a través de POST, se procese el
nombre de usuario y la contraseña contra el "groupfinder" que debe estar
registrada en la configuración.

En nuestra plantilla, buscamos el valor `logged_in`` de la clase "vista"
Usamos ésto para saber si hay un usuario logueado, de haberlo.
En la plantilla nosotros podemos escoher como mostrar el enlace
de inicio de sesión para los usuarios anónimos o el enlace de salida
para los usuarios logueados.


Crédito extra
============

#. ¿Cuál es la diferencia entre un usuario y un administrador?

#. ¿Puedo usar una base de datos detrás de mi ``groupfinder`` para buscar los administradores?

#. Una vez se inicia sesión, ¿toda la información centrada del usuario puede atascarse
    en cada solicitud? Usar ``import pdb; pdb.set_trace()`` para responder
   ésto.

.. seealso:: Vea también :ref:`security_chapter`,
   :ref:`AuthTktAuthenticationPolicy <authentication_module>`.