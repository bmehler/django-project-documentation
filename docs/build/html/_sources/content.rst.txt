Installation
============

Ein neues Python Projekt startet mit der Erstellung einer virtuellen Umgebung:

.. code-block:: console

   $  mkdir djangoProjectEnvironment
   $  cd djangoProjectEnvironment
   $  python3 -m venv env

Zum aktivieren der virtuellen Umgebung ist folgender Befehl notwendig:

.. code-block:: console

   $ . env/bin/activate

Nun können sie mit pip das Django Framework installieren:

.. code-block:: console

   (.venv) $ python -m pip install Django

Das Django Framework ist nun installiert. Öffnen wir die interactive Python Shell:

.. code-block:: console

   $ python3

Die Django Version kann wie folgt abgefragt werden:

.. code-block:: python
   
   >>> import django
   >>> print(django.get_version())
   5.1.3
   
Das Django Framework ist nun installiert.
