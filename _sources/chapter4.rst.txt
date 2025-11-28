Datenbank
=========

Dieses Kapitel beinhaltet nähere Informationen zur Datenbankanbindung. Die Mitarbeiterverwaltung
nutzt SQLite3 zur Speicherung der Datensätze. Die Version ist wie folgt abrufbar:

.. code-block:: console

   (env) $ sqlite3 -version

Die Einstellungen für die jeweilige Datenbank findet sich unter *source/settings.py* und sieht in unserem Fall wie folgt aus:

.. code-block:: python

   DATABASES = {
       'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': BASE_DIR / 'db.sqlite3',
       }
   }

Die Standard Tabellen von Django können mit dem Befehl:

.. code-block:: console

   (env) $ cd djangoProject
   (env) $ python manage.py migrate

Die erstellten Django Standardtabellen können z.b. mit dem DB Browser
für SQLite bequem administriert werden.

Nun werden die Models unserer Applikation erstellt. Hierzu erstellen wir folgenden
Code in der Datei *employees/models.py*.

.. code-block:: python

   from django.db import models

   class Login(models.Model):
       username = models.CharField(max_length=200)
       password = models.CharField(max_length=200)
       role = models.CharField(max_length=100)
       pub_date = models.DateTimeField("date published")    

   class Employees(models.Model):
       surname = models.CharField(max_length=200)
       name = models.CharField(max_length=200)
       position = models.CharField(max_length=200)
       salary = models.DecimalField(max_digits=10, decimal_places=2)
       pub_date = models.DateTimeField("date published")  

Das Model bildet die Grundlage zur Anlage der benötigten Tabellen in der Datenbank.
In einem nächsten Schritt wird zunächst die Datei 0001_inital.py aus dem Model generiert. Die geschieht wie folgt:

.. code-block:: console

   (env) $ python manage.py makemigrations employees

Nun können wir den SQL-Code generieren.

.. code-block:: console

   (env) $ python manage.py sqlmigrate employees 0001

In einem nächsten Schritt legen wir die beiden Datenbanktabellen an. Hierzu benötigen
wir folgenden Befehl:

.. code-block:: console
   
   (env) $ python manage.py migrate

Nun können wir das Ergebnis in unserer Datenbank einsehen. Es wurden zwei 
Datenbanktabellen angelegt.

Im Anschluss möchte ich in die beiden Datenbanktabellen Datensätze einfügen.
Hierzu verwende ich die interactive Python shell:

.. code-block:: console

   (env) $  python manage.py shell

.. code-block:: python

   >>> from employees.models import Login, Employees
   >>> Employees.objects.all()
   <QuerySet []>

   >>> from django.utils import timezone
   >>> e = Employees(surname="Muster", name="Max", position="Accounting", salary=3000, pub_date=timezone.now()) 
   >>> e.save()
   >>> e.id

.. important::

   Ein truncate sieht bei einer SQLite Datenbank wie folgt aus:

    .. code-block:: sql

        DELETE FROM employees_employees;
        UPDATE SQLITE_SEQUENCE SET seq = 0 WHERE name = 'employees_employees';
        VACUUM;