Fontawesome
===========

Nun wollen wir noch Icons in der Mitarbeiterverwaltung einbinden.
Hierzu ergänzen wir die Datei *base.html*

.. code-block:: python

    <html>
        <head>
            <title>Mitarbeiter App</title>
            {% load static %}
            ...
            <link rel="stylesheet" href="{% static 'employees/fontawesome/css/all.min.css' %}">
            <link rel="stylesheet" href="{% static 'employees/styles.css' %}">
        </head>

        <body>
            ...
        </body>
    </html>

Die Fontawesome Dateien können über das Github Repository herunterladen werden.

.. important::

    Man benötigt zum einen die Datei *all.min.css* und den Ordner */webfonts*.
    Die Datei *all.min.css* wird im Ordner css gespeichert.
    
    Die Datei Struktur sieht dann wie folgt aus:

    - static/employees/fontawesome/css/all.min.css
    - static/employees/fontawesome/webfonts

Ein Beispiel für ein Icon in der Mitarbeiterverwaltung.

.. code-block:: html

    <i class="fa-solid fa-user-group"></i>