Employee
========

In unserer Mitarbeiterverwaltung haben wir folgende Views:

.. note::

    - Employees *index* page  -> zeigt die Startseite mit den Datensätzen an
    - Employees *create* page -> hier können Datensätze angelegt werden
    - Employees *update* page -> aktualisiert einen Mitarbeiterdatensatz
    - Employees *delete* page -> löscht einen Mitarbeiterdatensatz

index.view
----------

Im Kapitel *Vorbereitungen* wurde bereits ein *index.view* angelegt. Dieser wird nun
bearbeitet.

*employees/views.py*

.. code-block:: python
   
   from django.shortcuts import render

   from .models import Employees

   def index(request):
    -  hello = 'Hello Django with Bootstrap!'
    -  context = { 
    -      'hello' : 'Hello Django with Bootstrap!'
    
    +  employees_list = Employees.objects.all()
    +  context = {"employees_list": employees_list}
    }
    return render(request, "employees/index.html", context)

Für eine Ausgabe der Employees im Frontend muss man das Template
entsprechend anpassen.

*employees/templates/employees/index.html*

.. code-block:: python

   - {{ hello }}

   + {% if employees_list %}
   +     <ul>
   +     {% for employee in employees_list %}
   +         <li>{{ employee.name }}</li>
   +     {% endfor %}
   +     </ul>
   + {% else %}
   +     <p>No employees are available.</p>
   + {% endif %}

Öffnen wir nun den Development Server so erscheint in der Ausgabe::

    Max


Wir möchten die Mitarbeiterdaten strukturiert in einer Tabelle augeben.
Hierzu passen wir das Template wie folgt an:

*employees/templates/employees/index.html*

.. code-block:: python

   {% load static %}

    <link rel="stylesheet" href="{% static 'employees/bootstrap.css' %}">

    <div class="container">
        <div class="row">
            <div class="col-lg-12">
                <table class="table">
                    <thead>
                        <tr>
                        <th scope="col">id</th>
                        <th scope="col">Name</th>
                        <th scope="col">Surname</th>
                        <th scope="col">Position</th>
                        <th scope="col">Salary</th>
                        <th scope="col">Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        {% if employees_list %}
                        <tr>
                            {% for employee in employees_list %}
                                <td>{{ employee.id }}</td>
                                <td>{{ employee.name }}</td>
                                <td>{{ employee.surname }}</td>
                                <td>{{ employee.position }}</td>
                                <td>{{ employee.salary }}</td>
                                <td>
                                    <a href="" class="btn btn-sm btn-primary">Create</a>
                                    <a href="" class="btn btn-sm btn-success">Update</a>
                                    <a href="" class="btn btn-sm btn-danger">Delete</a>
                                </td>
                            {% endfor %}
                        </tr>
                        {% else %}
                            <p>No employees are available.</p>
                        {% endif %}
                    </tbody>
                </table>
            </div>
        </div>
    </div>

Das Django Framework beinhaltet eine Templatestruktur. Diese wollen wir nun aufbauen.
Zunächst erstellen wir die Datei *employees/templates/base.html*. Diese enthält folgendes HTML-Konstrukt:

.. code-block:: python
    
    <html>
        <head>
            <title>Mitarbeiter App</title>
            {% load static %}
            <link rel="stylesheet" href="{% static 'employees/bootstrap.css' %}">
        </head>

        <body>
        {% block content %}{% endblock content %}
        </body>
    </html>

Das Coding in der Datei *employees/templates/employees/index.html* sieht nach der Anpassung wie folgt aus:

.. code-block:: python

    {% extends "base.html" %}

    {% block content %}
    <div class="container">
        <div class="row">
            <div class="col-lg-12">
                <table class="table">
                    <thead>
                        <tr>
                        <th scope="col">id</th>
                        <th scope="col">Name</th>
                        <th scope="col">Surname</th>
                        <th scope="col">Position</th>
                        <th scope="col">Salary</th>
                        <th scope="col">Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        {% if employees_list %}
                        {% for employee in employees_list %}
                        <tr>
                            <td>{{ employee.id }}</td>
                            <td>{{ employee.name }}</td>
                            <td>{{ employee.surname }}</td>
                            <td>{{ employee.position }}</td>
                            <td>{{ employee.salary }}</td>
                            <td>
                                <a href="{% url 'update' employee.id %}" class="btn btn-sm btn-primary">Update</a>
                                <a href="{% url 'delete' employee.id %}" class="btn btn-sm btn-danger">Delete</a>
                            </td>
                        {% endfor %}
                        </tr>
                        {% else %}
                            <p>No employees are available.</p>
                        {% endif %}
                    </tbody>
                </table>
            </div>
        </div>
    </div>
    {% endblock content %}

urlpatterns
-----------

Die Mitarbeiterverwaltung beinhaltet neben dem *index.view noch
andere *Views*. Im Kapitel *Vorbereitungen* haben wir bereits ein
*urlpattern* für den *index.view*. Nun legen wir weitere *urlpatterns* an.

.. code-block:: python

    app_name = "employees"
    urlpatterns = [
        ...
        path("create/", views.create, name="create"),
        # ex: /employees/create/
        path("update/<int:employees_id>", views.update, name="update"),
        # ex: /employees/update/5
        path("delete/<int:employees_id>", views.delete, name="delete"),
        # ex: /employees/delete/5
    ]

In der Datei *employees.views.py* legen wir die einzelnen Methoden an.

.. code-block:: python

    def index(request):
        ...

    def create(request):
        return ''

    def update(request):
        return ''

    def delete(request):
        return ''

create.view
-----------

Nun legen wir einen *create.view* an. Ein neuer Datensatz wird über einen *Create Button* im *index.view* angelegt.

*employees/templates/employees/index.html*

.. code-block:: python

    ...
    </table>
    <a href="{% url 'create' %}" class="btn btn-lg btn-outline-primary">Create</a>

In einem nächsten Schritt legen wir die Datei *create.html* unter templates an.

*employees/templates/employees/create.html*

.. code-block:: python
   
    {% extends "base.html" %}
    {% block content %}
    <form action={% url 'create' %} method="POST">
    {% csrf_token %}
        <div class="container mt-5">
            <h2>Create Employee</h2>
            <div class="row">
                <div class="col-lg-12">
                {% for field in form %}
                    <div class="mb-3">
                        {{ field.errors }}
                        {{ field.label_tag }} {{ field }}
                    </div>
                    {% endfor %}
                    <button type="submit" class="btn btn-outline-primary">Create</button>
                </div>
            </div>
        </div>
    </form>
    {% endblock %}

Die passende *create Methode* in der Datei *employees/views.py* sieht wie folgt aus:

.. code-block:: python

   def create(request):
       if request.method == 'POST':
            form = EmployeesForm(request.POST)
            if not form.is_valid():
                context = {
                    'form' : form
                }
                return render(request, "employees/create.html", context)
            else:
                e = Employees(name=form.cleaned_data['employees_name'], surname=form.cleaned_data['employees_surname'], position=form.cleaned_data['employees_position'], salary=form.cleaned_data['employees_salary'], pub_date=timezone.now())
                e.save()
                return redirect('/employees')
        else:
            form = EmployeesForm()
            context = {
                'form': form,
            }
            return render(request, "employees/create.html", context)   

Um die Formulareingaben zu validieren benötigen wir die Datei *employees.forms.py*. Diese hat folgenden Inhalt:

.. code-block:: python

    from django import forms
    from django.core.exceptions import ValidationError
    import re

    class EmployeesForm(forms.Form):
        employees_name = forms.CharField(label="Name")
        employees_name.widget.attrs.update({'class': 'form-control'})
        
        def clean_employees_name(self):
            data = self.cleaned_data['employees_name']
            if len(data) < 5:
                raise ValidationError("Please enter at least 5 characters!")
            return data
        
        employees_surname = forms.CharField(label="Surname")
        employees_surname.widget.attrs.update({'class': 'form-control'})

        def clean_employees_surname(self):
            data = self.cleaned_data['employees_surname']
            if len(data) < 5:
                raise ValidationError("Please enter at least 5 characters!")
            return data

        employees_position = forms.CharField(label="Position")
        employees_position.widget.attrs.update({'class': 'form-control'})

        def clean_employees_position(self):
            data = self.cleaned_data['employees_position']
            if len(data) < 5:
                raise ValidationError("Please enter at least 5 characters!")
            return data

        employees_salary = forms.CharField(label="Salary")
        employees_salary.widget.attrs.update({'class': 'form-control'})

        def clean_employees_salary(self):
            data = self.cleaned_data['employees_salary']
            if not re.match('[0-9]', data):
                raise ValidationError("Please enter only numbers!")
            return data

update.view
-----------

Beim update von Datensätzen legen wir zu Beginn das Template an.

*employees/templates/employees/update.html*

.. code-block:: python

    {% extends "base.html" %}
    {% block content %}
    <form action="" method="POST">
    {% csrf_token %}
        <div class="container mt-5">
            <h2>Update Employee</h2>
            <div class="row">
                <div class="col-lg-12">
                {% for field in form %}
                    <div class="mb-3">
                        {{ field.errors }}
                        {{ field.label_tag }} {{ field }}
                    </div>
                    {% endfor %}
                    <button type="submit" class="btn btn-outline-primary">Update</button>
                </div>
            </div>
        </div>
    </form>
    {% endblock %}  

Die update action in views.py beinhaltet folgendes:

.. code-block:: python

    def update(request, employees_id):
        if request.method == 'POST':
            form = EmployeesForm(request.POST)
            if not form.is_valid():
                context = {
                    'form' : form
                }
                return render(request, "employees/update.html", context)
            else:
                Employees.objects.filter(id = employees_id).update(name=form.cleaned_data['employees_name'], surname=form.cleaned_data['employees_surname'], position=form.cleaned_data['employees_position'], salary=form.cleaned_data['employees_salary'], pub_date=timezone.now())
                return redirect('/employees')
        else:
            form = EmployeesForm()
            e = Employees.objects.get(pk=employees_id)
        
            form.initial['employees_name'] = e.name
            form.initial['employees_surname'] = e.surname
            form.initial['employees_position'] = e.position
            form.initial['employees_salary'] = e.salary

            context = {
                'form': form,
            }

            return render(request, "employees/update.html", context)
            
delete.view
-----------

Zum Löschen von Datensätzen legen wir zu Beginn das Template an.

*employees/templates/employees/delete.html*

.. code-block:: python

    {% extends "base.html" %}
    {% block content %}
    <form action="" method="POST">
    {% csrf_token %}
        <div class="container mt-5">
            <h2>Update Employee</h2>
            <div class="row">
            <div class="col-lg-12">
                <div class="card" style="width: 30rem;">
                    <div class="card-header">
                        Delete Employee
                    </div>
                    <ul class="list-group list-group-flush">
                        <li class="list-group-item">Are you sure to delete employee {{ employee.name }} {{ employee.surname }}.</li>
                        <li class="list-group-item">
                            <button type="submit" class="btn btn-outline-danger float-end me-1">I am sure</button>
                            <a href="{% url 'index' %}" class="btn btn-success float-end me-1">I am not sure</a>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
        </div>
    </form>
    {% endblock %} 

Die entsprechend delete action in der Datei *employees/views.py* sieht wie folgt aus:

.. code-block:: python

    def delete(request, employees_id):
        if request.method == 'POST':
            e = Employees.objects.get(pk=employees_id)
            e.delete()
            return redirect('/employees')
        else: 
            e = Employees.objects.get(pk=employees_id)
            context = {
                'employee': e,
            }   
            return render(request, "employees/delete.html", context)