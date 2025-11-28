Vorbereitungen
==============

Beginnen möchte ich das Tutorial mit der Erstellung einer index action
zusammen mit einem index view und der Einbindung des *Frameworks Bootstrap*.

.. note::

   Folgende Dateien müssen hierzu verändert werden:

   - employees/views.py
   - employees/urls.py

   - source/urls.py
   - source/settings.py
   
   Zur Einbindung des Frameworks Bootstrap sind folgende Dateien betroffen:

   - employees/static/employees/bootstrap.css
   - employees/templates/employees/index.html

*employees/views.py*

.. code-block:: python
   
   from django.shortcuts import render

   def index(request):
       hello = 'Hello Django with Bootstrap!'
       context = { 
           'hello' : 'Hello Django with Bootstrap!'
    }
    return render(request, "employees/index.html", context)

*employess/urls.py*

.. code-block:: python

   from django.urls import path

   from . import views

   urlpatterns = [
       path("", views.index, name="index"),
   ]

*source/urls.py*

.. code-block:: python

   from django.contrib import admin
   from django.urls import include, path

   urlpatterns = [
       path("employees/", include("employees.urls")),
   ]

*source/settings.py*

.. code-block:: python

   INSTALLED_APPS = [
       'employees.apps.EmployeesConfig',
       ...

Die Templates werden in Django im Ordner templates/employees/ gespeichert.
Wir legen das Verzeichnis mit der Datei index.html an und fügen folgenden Code ein::

    {{ hello }}

In einem nächsten Schritt legen wir den Ordner static/employees/ an.
Dieser Ordner enthält nach Anlage die Datei bootstrap.css. Die bootstrap.css kann
auf der Website heruntergelanden und eingefügt werden. Die Datei index.html sieht 
aktualisiert nun wie folgt aus::

    {% load static %}

    <link rel="stylesheet" href="{% static 'employees/bootstrap.css' %}">
    {{ hello }}

Öffnen wir den Development Server *http://127.0.0.1:8000/employees/* so erscheint.::

    Hello Django with Bootstrap!

Der Text wurde mit der Bootstrap formatiert.
