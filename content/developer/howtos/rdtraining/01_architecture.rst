.. _howto/rdtraining/01_architecture:

================================
Chapter 1: Architecture Overview
================================

Multitier application
=====================

Odoo sigue una `arquitectura multinivel`_, lo que significa que la presentación, el negocio
la lógica y el almacenamiento de datos están separados. Más específicamente, 
utiliza una arquitectura de tres niveles
(imagen de Wikipedia):

.. image:: 01_architecture/three_tier.svg
    :align: center
    :alt: Three-tier architecture

El nivel de presentación es una combinación de HTML5, JavaScript y CSS. 
El nivel lógico es exclusivamente escrito en Python, mientras que el 
nivel de datos solo admite PostgreSQL como RDBMS.

Según el alcance de su módulo, el desarrollo de Odoo se puede realizar en cualquiera de estos niveles.
Por eso, antes de seguir adelante, puede ser una buena idea `refrescar`_ la memoria si no tienes
un nivel intermedio en estos temas.

Para realizar este tutorial, necesitará un conocimiento muy básico de HTML y un nivel intermedio.
nivel de Python. Los temas avanzados requerirán más conocimiento en las otras materias. Existen
muchos tutoriales de libre acceso, por lo que no podemos recomendar uno sobre otro ya que depende
en tu fondo.

Como referencia, este es el `Tutorial de Python`_ oficial.

.. note::
Desde la versión 15.0, Odoo está haciendo una transición activa para usar su propio OWL desarrollado internamente.
framework <https://odoo.github.io/owl/>`_ como parte de su nivel de presentación. El JavaScript heredado
framework todavía es compatible, pero quedará obsoleto con el tiempo. 
Esto se discutirá más adelante en temas avanzados.

Odoo modules
============

Both server and client extensions are packaged as *modules* which are
optionally loaded in a *database*. A module is a collection of functions and data that target a
single purpose.

Odoo modules can either add brand new business logic to an Odoo system or
alter and extend existing business logic. One module can be created to add your
country's accounting rules to Odoo's generic accounting support, while
a different module can add support for real-time visualisation of a bus fleet.

Everything in Odoo starts and ends with modules.

Terminology: developers group their business features in Odoo *modules*. The main user-facing
modules are flagged and exposed as *Apps*, but a majority of the modules aren't Apps. *Modules*
may also be referred to as *addons* and the directories where the Odoo server finds them
form the ``addons_path``.

Composition of a module
-----------------------

An Odoo module **can** contain a number of elements:

:ref:`Business objects <reference/orm>`
    A business object (e.g. an invoice) is declared as a Python class. The fields defined in
    these classes are automatically mapped to database columns thanks to the
    :abbr:`ORM (Object-Relational Mapping)` layer.

:ref:`Object views <reference/views>`
    Define UI display

:ref:`Data files <reference/data>`
    XML or CSV files declaring the model data:

    * :ref:`views <reference/views>` or :ref:`reports <reference/reports>`,
    * configuration data (modules parametrization, :ref:`security rules <reference/security>`),
    * demonstration data
    * and more

:ref:`Web controllers <reference/controllers>`
    Handle requests from web browsers

Static web data
    Images, CSS or JavaScript files used by the web interface or website

None of these elements are mandatory. Some modules may only add data files (e.g. country-specific
accounting configuration), while others may only add business objects. During this training, we will
create business objects, object views and data files.
:ref:`Web controllers <howto/rdtraining/G_website>` and
:ref:`static web data <howto/rdtraining/I_jswidget>` are advanced topics.

Module structure
----------------

Each module is a directory within a *module directory*. Module directories
are specified by using the :option:`--addons-path <odoo-bin --addons-path>`
option.

An Odoo module is declared by its :ref:`manifest <reference/module/manifest>`.

When an Odoo module includes business objects (i.e. Python files), they are organized as a
`Python package <https://docs.python.org/3/tutorial/modules.html#packages>`_
with a ``__init__.py`` file. This file contains import instructions for various Python
files in the module.

Here is a simplified module directory:

.. code-block:: bash

    module
    ├── models
    │   ├── *.py
    │   └── __init__.py
    ├── data
    │   └── *.xml
    ├── __init__.py
    └── __manifest__.py

Odoo Editions
=============

Odoo is available in `two versions`_: Odoo Enterprise (licensed & shared sources) and Odoo Community
(open-source). In addition to services such as support or upgrades, the Enterprise version provides extra
functionalities to Odoo. From a technical point-of-view, these functionalities are simply
new modules installed on top of the modules provided by the Community version.

Ready to start? Before writing actual code, let's go to the
:ref:`next chapter <howto/rdtraining/02_setup>` to review the Odoo installation process. Even if
Odoo is already running on your system, we strongly suggest you go through this chapter
to make sure we start on the same page during the development of our new application.

.. _multitier architecture:
    https://en.wikipedia.org/wiki/Multitier_architecture

.. _Python tutorial:
    https://docs.python.org/3.7/tutorial/

.. _two versions:
    https://www.odoo.com/page/editions
