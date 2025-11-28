Login
=====

In unserer Mitarbeiterverwaltung möchten wir nun einen Login erstellen.
Die Datenbanktablle employees_login wurde bereits angelegt.


.. note::

   In der interactiven Shell können wir die Authentifizierung vorab überprüfen.
   
.. code-block:: python

    >>> from django.utils import timezone
    >>> from django.contrib.auth.hashers import check_password
    >>> l = Login(username="admin", password="pbkdf2_sha256$36000$c3NvhDVRtXWQ$sHjXq/4E4b+wz3BsJDCEpRfRcUutoH5DsNNw+oeu/Yo=", role="admin", pub_date=timezone.now())
    >>> l.save()
    >>> l.password
    >>> 'pbkdf2_sha256$36000$c3NvhDVRtXWQ$sHjXq/4E4b+wz3BsJDCEpRfRcUutoH5DsNNw+oeu/Yo='
    >>> check_password('admin', l.password)
    True   

Navbar
------ 

Im Folgenden wollen wir eine Navbar einbinden. Hierzu
nehmen wir diese Anpassungen vor.

*employees/templates/base.html*

.. code-block:: python

    <html>
        <head>
            {% load static %}
            ...
            <link rel="stylesheet" href="{% static 'employees/styles.css' %}">
        </head>

        <body>
        {% include "layout/navigation.html" %} 
        ...
        </body>
    </html>

Wir erstellen die Datei *navigation.html* mit dem Inhalt.

.. code-block:: python

    <nav class="navbar navbar-expand-lg bg-body-tertiary mb-5">
        <div class="container">
            <a class="navbar-brand" href="#">Employees App</a>
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                <li class="nav-item">
                <a class="nav-link active" aria-current="page" href="{% url 'index' %}">Home</a>
                </li>
            </ul>
            <div class="d-flex">
                <a href="{% url 'login' %}" class="btn btn-outline-primary">Login</a>
                <a href="{% url 'logout' %}" class="btn btn-primary">Logout</a>
            </div>
            </div>
        </div>
    </nav>

Wir ergänzen in einem nächsten Schritt die *urlpatterns*

.. code-block:: python

    urlpatterns = [
        ...
        path("login/", views.login, name="login"),
        path("logout/", views.logout, name="logout"),
    ]

login.views
-----------

Das Login Formular wird in der Datei *styles.css* formatiert.

.. code-block:: css

    html,
    body {
        height: 100%;
    }

    .form-signin {
        max-width: 330px;
        padding: 1rem;
    }

    .form-signin .form-floating:focus-within {
        z-index: 2;
    }

    .form-signin input[type="email"] {
        margin-bottom: -1px;
        border-bottom-right-radius: 0;
        border-bottom-left-radius: 0;
    }

    .form-signin input[type="password"] {
        margin-bottom: 10px;
        border-top-left-radius: 0;
        border-top-right-radius: 0;
    }

Für den Login ist folgendes Template notwendig.

*employees/templates/employees/login.html*

.. code-block:: python

    {% extends "base.html" %}
    {% block content %}
    <main class="form-signin w-100 m-auto">
        <form action="{% url 'login' %}" method="POST">
        {% csrf_token %}
            <h1 class="h3 mb-3 fw-normal text-center">Sign in</h1>
            <div class="form-floating">
            <input type="text" name="username" class="form-control" id="floatingInput" placeholder="Username">
            <label for="floatingInput">Username</label>
            </div>
            <div class="form-floating">
            <input type="password" name="password" class="form-control" id="floatingPassword" placeholder="Password">
            <label for="floatingPassword">Password</label>
            </div>
            <button class="btn btn-outline-primary w-100 py-2" type="submit">Sign in</button>
            <p class="mt-5 mb-3 text-body-secondary text-center">Bernhard Mehler &copy; 2024</p>
        </form>
    </main>
    {% endblock content %}

Die *Login action* beinhaltet folgendes.

.. code-block:: python

    def login(request):
    if request.method == 'POST':
        username = request.POST["username"]
        password = request.POST["password"]
        l = Login.objects.get(username=username)
        if check_password(password, l.password):
            request.session["login_id"] = l.id
            request.session["login_role"] = l.role
            return redirect('/employees')
        else:
            context = {
                'error': 'Username or Password invalid!',
            }  
            return render(request, "employees/login.html", context)
    else:
        return render(request, "employees/login.html")

und die *Logout action*

.. code-block:: python

    def logout(request):
        try:
            del request.session["login_id"]
            del request.session["login_role"]
        except KeyError:
            pass
        return redirect('/employees')