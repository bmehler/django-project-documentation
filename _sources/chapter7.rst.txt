Debug Toolbar
=============

Als Hilfe bei der Entwicklung mit Django bietet 
die Debug Toolbar verschiede Hinweise über 
den aktuellen Verlauf der Anwendung an.

Installation
------------

Zuerst Installieren wir die Debug Toolbar mit pip.

.. code-block:: console

    (env) $ cd djangoProject
    (env) $ python -m pip install django-debug-toolbar

*settings.py*

Vorbereitungen
--------------

.. code-block:: python

    INSTALLED_APPS = [
        # ...
        "django.contrib.staticfiles",
        # ...
    ]

    STATIC_URL = "static/"

    TEMPLATES = [
        {
            "BACKEND": "django.template.backends.django.DjangoTemplates",
            "APP_DIRS": True,
            # ...
        }
    ]

Installation
------------

.. code-block:: python

    INSTALLED_APPS = [
        "debug_toolbar",
        'employees.apps.EmployeesConfig',
        ...
    ]

Urls
----

.. code-block:: python

    from django.urls import include, path
    from debug_toolbar.toolbar import debug_toolbar_urls

    urlpatterns = [
        # ... the rest of your URLconf goes here ...
    ] + debug_toolbar_urls()

Middleware
----------

.. code-block:: python

    MIDDLEWARE = [
        "debug_toolbar.middleware.DebugToolbarMiddleware",
        # ...
    ]

Interne IPs
-----------

.. code-block:: python

    INTERNAL_IPS = [
        "127.0.0.1",
    ]