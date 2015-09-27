===========================================
21: Proteger recursos con "Autorización"
===========================================

Asignar declaraciones de seguridad para recursos que describan los permisos
requeridos para realizar una operación.

Fondo
==========

Nuestra aplicación tendrá URLs que permitirán a través del navegador
al usuario agregar/editar/borrar contenido. Es tiempo de agregar seguridad
a la aplicación. Vamos a proteger nuestra vista de agregar/editar requeriendo
un inicio de sesión (nombre de usuario del ``editor`` y contraseña del
``editor``). Vamos a permitir que otras vistas continúen trabajando
sin una contraseña.


Objetivos
==========

- Introducción a los conceptos de autenticación, autorización,
permiso, y listas de acceso de control(ACLs) en Pyramid. 

- Hacer un :term:`root factory` que retorne una instancia de nuestra
clase para la parte superior de la aplicación.

- Asignar declaraciones de seguridad para nuestro recurso raíz

- Agregar un permisos en una vista implicada.

- Proveer un :term:`Forbidden view` para manejar las visitas a una 
URL sin el permiso adecuado.


Pasos
=====

#. Vamos a usar el paso de autenticación como nuestro punto de inicio:

   .. code-block:: bash

    $ cd ..; cp -r authentication authorization; cd authorization
    $ $VENV/bin/python setup.py develop

#. Comenzaremos por cambiar: ``authorization/tutorial/__init__.py`` para
   especificar una raíz al :term:`configurator`:

   .. literalinclude:: authorization/tutorial/__init__.py
    :linenos:

#. Eso significa que tenemos que que vamos a implementar:
   ``authorization/tutorial/resources.py``

   .. literalinclude:: authorization/tutorial/resources.py
    :linenos:

#. Cambiar ``authorization/tutorial/views.py`` para requerir ``edit``
   permiso en la vista ``hello`` e implementar la prohibición en la vista:

   .. literalinclude:: authorization/tutorial/views.py
    :linenos:

#. Ejecuta tu aplicación Pyramid usando:

   .. code-block:: bash

    $ $VENV/bin/pserve development.ini --reload

#. Abrir http://localhost:6543/ en el navegador.

#. Si apareces con la sesión iniciada, click en el link "salir".

#. Visita http://localhost:6543/howdy en una navegador. Se te pedirá
   que inicies sesión


Análisis
========

Este simple tutorial puede reducirse a lo siguiente:

- Una vista puede requerir un *permiso* (``edit``)

- El contexto para nuestra vista (el ``Root``) tiene una lista de
  acceso de control. (ACL)

- Esta ACL dice que el permiso para ``edit`` está disponible en, ``Root``
  para el grupo ``group:editors`` *principal*

- El  registro ``groupfinder`` responderá si un usuario en particular
  (``editor``) pertenece a un grupo en especial (``group:editors``)

En resumen: ``hello`` requiere permisos para ``edit`` , ``Root`` dice el
``group:editors`` tiene permisos para ``edit``.

Claro, esto solo aplica en: ``Root``. En otra parte del sitio
(a.k.a. *contexto*) podría tener un diferente ACL.

Sino has iniciado sesión y visitas ``/howdy``, necesitarás hacerlo en
la pantalla de inicio de sesión. Cómo sabe Pyramid cuál es la página de inicio
de sesión? Nosotros explícitamente se lo dijimos en la vista ``login`` usando el
decorador ``@forbidden_view_config`` en dicha vista.

Crédito extra
============

#. Tengo que poner un ``renderer`` en mi decorador ``@forbidden_view_config``?

#. Tal vez le gustaría experimentar con no tener suficientes permisos
   (forbidden) como para editar. ¿Cómo puedes hacer para cambiar ésto?

#. Tal vez queremos almacenar declaraciones de seguridad en una base de datos y
    permitir la edición a través de un navegador. ¿Cómo puede hacerse esto?

#. ¿Qué pasa si queremos diferentes declaraciones de seguridad en diferentes tipos de
    objetos? O en el mismo tipo de objetos, pero en diferentes partes de una
    Jerarquía URL?
