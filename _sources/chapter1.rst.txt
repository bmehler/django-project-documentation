Getting started
===============

Ein Django Projekt wir wie folgt initalisiert:

.. code-block:: console

   (env) $ cd djangoProjectEnvironment
   (env) $ mkdir djangoProject

   (env) $ django-admin startproject source djangoProject
   (env) $ cd djangoProject

Django verfügt über einen Development Server. 
Folgender Befehl startet den Server:

.. code-block:: console  

   (env) $ python manage.py runserver

Im Browser sehen sie nun unter *127.0.0.1:8000* die Startseite des Django Projekts.

In einem nächsten Schritt folgt die Erstellung der Django app:

.. code-block:: console

   (env) $ cd djangoProject
   (env) $ python manage.py startapp employees

Es wurde ein Projektverzeichnis mit dem Namen employees erstellt.
Dieses beinhaltet die spezifischen Dateien des Django Projektes und sind
nach dem MVC Prinzip organisiert.
