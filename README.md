# Codecademy
# Build a python web apps with Django

## HTML file vs DOM

HTML, JavaScript and CSS work together to style a web page with the template of a HTML syntax

![DOM.JPG](7a334b19-3a75-45ba-ba18-45e9cfbf3e3f.JPG)

## Storing Data

You’ve probably heard that data is a big deal. By some measures, 90% of the world’s data has been generated in just the past two years. From a stored credit card number on an e-commerce site to the timestamp when you pause a video on a streaming platform, modern web applications collect and use large amounts of data. For that data to be useful, it must be organized and stored somewhere.

The back-ends of modern web applications include one or more databases. Databases are systems used to store and organize information so that it can be retrieved and updated efficiently. There are many types of databases, but they are commonly grouped into two broad categories: relational databases and non-relational databases (also known as NoSQL databases).

Relational databases organize data into tables made up of rows and columns. Each table typically represents a type of data, such as users, products, or orders. SQL (Structured Query Language) is commonly used to access and modify data stored in relational databases. Popular relational databases include MySQL and PostgreSQL.

Non-relational databases store data using other structures, such as key-value pairs, documents, or tree-like structures. These databases are often used when applications need flexible data formats or very large-scale data storage. Popular NoSQL databases include MongoDB and Redis.

In addition to storing the data, the 
back-end
Preview: Docs Loading link description
 must also include code that can read, create, update, and delete that data when requests are made by users or applications.

![Storing_data.JPG](b9357338-51c0-4a35-a990-7efc511ad60d.JPG)

## What is an API?

When a user navigates to a specific item for sale on an e-commerce site, the price listed for that item is stored in a database back-end does involves reading, creating, updating, or deleting data stored in a database.

To provide consistent ways for applications to interact with this data, a back-end often exposes a web API. API server and interact with its data, usually through an HTTP request–response cycle.

Instead of simply requesting a webpage, these requests tell the server what action should be performed on the data. For example, an API request might create new data, retrieve existing data, update data, or delete data. The server processes the request, interacts with the database if needed, and then returns a response.

Let’s revisit the example of making an online purchase. When a user submits an order, the browser loads an order confirmation page. At the same time, the front-end sends a request to the web API to record the order details and update the product inventory in the database.

Some web APIs are public, meaning other developers can use them to access certain features or data. For example, Instagram provides APIs that developers can use to interact with parts of the Instagram platform. Other APIs are internal, meaning they are only used by the application itself to allow different parts of the system to communicate.

## Different back-end stacks

![Back-end_stacks.JPG](fad8fe4f-d360-4da6-9673-2963f7b6b596.JPG)

- The front-end of a website or application consists of the HTML, CSS, JavaScript, and static assets sent to a client, like a web browser.
- A web server is a process running on a computer somewhere that listens for incoming requests for information over the Internet and sends back responses.
- Storing, accessing, and manipulating data is a large part of a web application’s back-end.
- Data is stored in databases, which can be relational databases or NoSQL databases.
- The server-side of a web application, sometimes called the application server, handles important tasks such as authorization and authentication.
- The back-end of a web application often has a web API which is a way of interacting with an application’s data through HTTP requests and responses.
- Together, the technologies used to build the front-end and back-end of a web application are known as the stack, and many different languages and frameworks can be used to build a robust back-end.

![Back-end.JPG](747678eb-6e49-4bbe-9e99-bb270d80a7c9.JPG)

# Start a Django project

**django-admin startproject projectname**

Para iniciar un proyecto en django

**python3 manage.py runserver 0.0.0.0:4001**

Para iniciar el servidor (La ip y el numero de puerto pueden cambiar)

**python3 manage.py migrate**

Para migrar todos los cambios no aplicados de la base de datos a la version actual

**python3 manage.py startapp myapp**

Para iniciar un directorio nuevo para una django app

## views.py

```python
from django.shortcuts import render
# Import HttpResponse here
from django.http import HttpResponse

# Create your views here.
def home(request):
  return HttpResponse("Welcome to the Vet's office!")
````

The resulting templates folder structure will look like this:



```python
myapp/
└── templates/
    └── myapp/
      └── mytemplate.html
```

## Creating a Django template

To place content generated from Django inside the HTML file, we need to turn our static HTML file into a template.

In the context of a web framework, templates are pages created with special markup that allows for backend data and commands to modify the contents of a page. Django employs a special syntax called Django Templating Language to distinguish itself from HTML, CSS, and JavaScript. That syntax in many template languages uses curly braces, sometimes referred to as handlebars, as a placeholder for data that is passed by Django.

In HTML, we use curly braces like this:

```html
<h1>Hello, {{name}}</h1>
````

When we call the view to render the template, we can use something called a context to tell Django what to replace in the template. The relationships in the context are referred to as a name/value pair. By default, a context is an empty dictionary. Our context for name will look like this inside the view function:

````python
context = {"name": "Junior"}
````

We then pass the context as an argument in the render function. The full view.py will look like this:

````python
from django.http import HttpResponse
from django.template import loader
def home(request):
  context = {"name": "Junior"}
  template = loader.get_template("app/home.html")
  return HttpResponse(template.render(context))
````

This would return a webpage that says “Hello, Junior” inside an \<h1> tag.

It’s quite common in Django to load templates, fill their context, and return an HttpResponse object with their rendered template. Django provides a shortcut for this pattern called the render() function! The render() function will do the work of loading the template and provide the contexts when they are passed as arguments.

Our example above can be rewritten with the shortcut function like this:

````python
from django.shortcuts import render

def home(request):
  context = {"name": "Junior"}
  return render(request, "app/home.html", context)
````

Note that we no longer need to import loader and HttpResponse when we use the render() function. The render() function takes the request object as its first argument, a template name as its second argument, and a dictionary as an optional third argument that passes the context variables to the template.

## Wiring up a View

An app’s URLconf is located in a file named urls.py inside the app’s directory. At the top of the urls.py we import the path object from django.urls and we import the view functions from views.py and add routes that direct to each of our view functions.

The urls.py will look like this:

````python
from django.urls import path
from . import views

urlpatterns = [
  path('', views.home),
  path('profile/', views.profile, name="profile")  
]
````

After the import statements is a list of patterns called urlpatterns, which contain the routes to each view function. Each route is provided as a path() object that has three arguments: the URL route as a 
string, the name of the function of the view, and an optional name used to refer to the view.


With the above example, when we navigate to the URL without any additional route, '', the home() view function will be called. Going to the URL ending with /profile will call the profile() view function.

## Templates

Django gives us Django Template Language (DTL) which lets us inject variables, logic, and control flow inside of our HTML - supercharging our HTML files to do so much more than provide static content.

In order to create templates, they have to be stored in the application in a folder called templates/. Another folder needs to be created inside of this templates/ folder that uses the same name of the application. All of the templates will go into this folder named after the application. The full file path to a template should look like this:


```python
projectname/
  └── appname/
        └── templates/
              └── appname/
                    └── first_template.html
```

## Creating a base template

Navigation bar could grow really large! This means each page that contains this code would continue to grow along with it. Django solves this issue of copying and pasting the same reused code from each template into something one template called a base template. Some elements that might go into the base template are: headings, navigation bars, footers, etc — these elements show up on most, if not all of the web pages for the application.

Once the common elements have been moved to base.html, we can start talking about adding page-specific content. Since the base.html will be used across several templates, we need to tell the application where we want our page-specific content to go. To do this, we add tags to the body of the base template. Tags are used to help extend the base template to other templates.

we just need to know that tags are created using the {% and %} symbols. Inside of these tags, we’ll be adding block content, and later another tag with the content endblock.

This creates a block that we can add code to in other templates. This block gives us the ability to later insert content that is specific to individual pages. It should look like this:

{% block content %}

{% endblock %}

## Extending the base template

**home.html**

````html
{% extends "vetoffice/base.html" %}
{% block content %}
<p>Welcome!</p>
{% endblock %}
````

**base.html**

````html
<!DOCTYPE html>
<head></head>
<body>
  <h1>Welcome to Vet Office!</h1>
  {% block content %}
  {% endblock %}
</body>
````

## Adding CSS to the template

We need a folder to store our CSS files, this folder will be in the application’s main folder and called static/. This folder will hold assets like pictures and CSS files. Another folder will be created inside of static/ that will be named after our application. The full path should look like the one below:


```python
projectname/
 └── appname/
     └── templates/
     └── static/
         └── appname/
             └── file.css
```

**style.css**

````css
p {
  color: red;
}
````

**home.html**

````html
{% extends 'vetoffice/base.html' %}
<!-- Add your code below: -->
{% load static %}

{% block head %}
<link rel="stylesheet" href="{% static 'vetoffice/style.css' %}">
{% endblock %}

{% block content %}
<p>Welcome!</p>
{% endblock %}
````

**base.html**

````html
<!DOCTYPE html>
<head>
  <!-- Add your head block below: -->
{% block head %}
{% endblock %}
</head>

<body>
  <h1>Welcome to Vet Office!</h1>
  {% block content %}
  {% endblock %}
</body>
````

## Variables in templates

We’ll cover the specifics of how views provide variables for templates in a later lesson. For now, we’ll just review the syntax for evaluating variables — two symbols are needed, {{ and }}, we call these symbols variable tags. When we add a variable in between variable tags, Django knows that we want the value of that variable from our views.py file.

````html
<p>{{ username }}</p>
````

Dictionaries and variable tags work well together. In a single variable tag, we can grab a 
dictionary and access all its properties! Imagine if we stored our user’s information in a dictionary named user:

````html
<p>{{ user.username }}</p>
````

````html
{% extends 'vetoffice/base.html' %}
{% load static %}

{% block head %}
  <link rel="stylesheet" href="{% static 'vetoffice/style.css' %}">
{% endblock %}

{% block content %}
  <!-- Edit the <p> element below -->
  <p>Welcome, {{ name }}</p>
{% endblock %}
````

## Conditionals in templates

These if statements help customize web pages without having to create separate templates for different instances. Imagine if we have an application that shows information for different US cities. Making individual templates for each city could take ages! Instead, we can use if statements to determine what city’s information to display.

An if statement in DTL is very similar to Python if statements. However, they consist of two necessary components and two optional components. The major components are:

- An if keyword is used in every if statement and its purpose is the same as in Python.
- An endif keyword is used to let DTL know that we are at the end of the if statement.

The two optional components are:

- elif - which is used if we want to check more than one condition within the if statement.
- else - which will execute whenever the if and all elifs evaluates as false. It will be the last thing included in an if statement before the endif.

To add an if statement to the template, we’ll need to set it up inside of tags. Remember, tags are the {% and %} symbols we used earlier for extending our base template to other templates. Generally, tags are used to tell the DTL that an expression needs to be executed or evaluated. There is no need to use separate variable tags when accessing a variable in normal tags. For instance, if we wanted to display attractions for New York or Los Angeles, we could use the following conditional:

````html
{% if city.name == "New York" %}
  <p>Attractions for New York City are</p>
  ...
{% elif city.name == "Los Angeles" %}
  <p>Attractions for Los Angeles are</p>
  ...
{% else %}
  <p>We currently do not have any attractions for that city</p>
{% endif %}
````

---

````html
{% extends "vetoffice/base.html" %}
{% load static %}

{% block head %}
  <link rel="stylesheet" href="{% static 'vetoffice/style.css' %}">
{% endblock %}

{% block content %}
  <p>Welcome, {{name}}!</p>
  <!-- Add your if statement below: -->
{% if pet.animal_type == "Dog" %}
  <p>The animal is a dog</p>
{% elif pet.animal_type == "Cat" %}
  <p>The animal is a cat</p>
{% else %}
  <p>The animal is not a dog or cat</p>
{% endif %}

{% endblock %}
````

## Loops in templates

When dealing with a dictionary in a Django template, we can save time by taking advantage of DTL’s for loop to iterate over individual items. Loops in DTL work like regular Python for loops but still require tags.

````html
{% for item in list_name %}
  <p>{{ item }}</p>
{% endfor %}
````

Inside the loop’s body, during each iteration, we can access the current key using the temporary variable key inside variable tags {{ }}. We’re free to manipulate the key as a variable using standard Python syntax. If our list contains dictionaries, we could even access the value of our dictionary if we change our loop:

````html
{% for key, value in dictionary_list %}
  <p>{{ key }} : {{ value }}</p>
{% endfor %}
````

Using loops, we can greatly reduce the amount of HTML we need to write to display a lot of information!

**views.py**

````python
from django.shortcuts import render

pets = [
  { "petname": "Fido", "animal_type": "dog"},
  { "petname": "Clementine", "animal_type": "cat"},
  { "petname": "Cleo", "animal_type": "cat"},
  { "petname": "Oreo", "animal_type": "dog"},
]

def home(request):
  context = {"name": "Djangoer", "pets": pets}
  return render(request, "vetoffice/home.html", context)
````

**home.html**

````html
{% extends 'vetoffice/base.html' %}
{% load static %}

{% block head %}
  <link rel="stylesheet" href="{% static 'vetoffice/style.css' %}">
{% endblock %}

{% block content %}
<p>Welcome, {{ name }}!</p>
<p>Here is the current list of patients:</p>
  <!-- Put the loop below here! -->
{% for pet in pets %}  
  <p>{{ pet.petname }} : {{ pet.animal_type }}</p>
{% endfor %}
{% endblock %}
````

## Adding URLs to a template

When navigating between pages using HTML, we need the entire URL to be written out in a \<a> element’s href attribute. With Django, rather than using the full URL we get a shortcut by using tags and the name of predefined paths! Later on, we’ll also cover how to pass along data in the URL, however, let’s first see the basic shortcut in action:

````html
<a href="{% url 'path_name' %}">Sample link</a>
````

When a path requires arguments to get to, like a username, it can be added to the href after the path. We won’t go into detail regarding this, but it would look like this:

````html
<a href="{% url 'path_name' username %}">User Profile</a>
````

**urls.py**

````python
from django.urls import path

from . import views

urlpatterns = [
   path('', views.home, name="home")
]
````

**base.html**

````html
<!DOCTYPE html>
<head>
  {% block head %}
  {% endblock %}
</head>

<body>
  <!-- Add your <a> below -->
  <a href="{% url 'home' %}">Vet Office</a> 
  <h1>Welcome to Vet Office!</h1>
  {% block content %}
  {% endblock %}
</body>
````

## Filters in templates

An example filter can be seen below:

````html
<p>{{ username|upper }}</p>
````

A filter with an argument can be seen here:

````html
{{ description|truncatewords_html:2 }}
````

The truncatewords_html filter requires an argument and will shorten text down to the integer supplied by our argument. In our case, we want to display 2 words max. Any other words in the description variable will be replaced with .... We were able to supply our argument after the filter name separated by a :

### Filters with arguments

````html
{% extends 'vetoffice/base.html' %}
{% load static %}

{% block head %}
  <link rel="stylesheet" href="{% static 'vetoffice/style.css' %}">
{% endblock %}

{% block content %}
<!-- Edit name below to use a filter -->
<p>{{ name|lower }}</p>
  <!-- Edit the loop below to use a filter -->
  {% for key in pets|dictsort:"petname" %}
    <p>pet name : {{ key.petname }}</p>
  {% endfor %}
{% endblock %}
````

## Models and databases

## Creating a Schema

![Creating_Schema.JPG](55a06366-bcb6-4b39-812d-c36113b01f86.JPG)

|Característica|Primary Key (PK)|Foreign Key (FK)|
|--------------|----------------|----------------|
|Identificación|Identifica un registro de forma única.|Identifica una relación con otra tabla.|
|Valores Nulos|No permitidos.|Permitidos (generalmente).|
|Valores Duplicados|No permitidos.|Permitidos.|
|Cantidad por tabla|Solo una.|Puede haber varias.|

Models in django are the tables in a database

**models.py**

````python
from django.db import models
````

To create a model

````python
class Flower(models.Model):
  ## Define attributes here
  pass
````

Notice that our model actually inherits from the Model parent class django.db.models.Model module. These models will later inform the database to use this schema to build its tables.

## Adding Models Fields

As we mentioned, models are used to represent real-life objects. We can mimic and create object attributes in our models using fields. Fields have names and are assigned a type. For example, a Flower model can have a petal_color field that expects a string:

````python
class Flower(models.Model):
  petal_color = models.CharField()
````

----

````python
class Flower(models.Model):
  petal_color = models.CharField()
  petal_number = models.IntegerField()
  # More attributes… 
````

https://docs.djangoproject.com/en/3.1/ref/models/fields/#model-field-types

We might also want to add constraints to our fields. For example, we might want our petal_color field to have a max length of 20 characters. We can supply an argument like so:

````python
class Flower(models.Model):
  petal_color = models.CharField(max_length=20)
  petal_number = models.IntegerField(default=0)
````

----

**models.py**

````python
from django.db import models

class Owner(models.Model):
  # Delete pass and add the Owner fields
  first_name= models.CharField(max_length=30)
  last_name= models.CharField(max_length=30)
  phone= models.CharField(max_length=30)

class Patient(models.Model):
  # Delete pass and add the Patient fields
  breed= models.CharField(max_length=200)
  pet_name= models.CharField(max_length=200)
  age= models.IntegerField(default=0)
````

## Primary Key, Foreign Key and Relationships

|Característica|Primary Key (PK)|Foreign Key (FK)|
|--------------|----------------|----------------|
|Identificación|Identifica un registro de forma única.|Identifica una relación con otra tabla.|
|Valores Nulos|No permitidos.|Permitidos (generalmente).|
|Valores Duplicados|No permitidos.|Permitidos.|
|Cantidad por tabla|Solo una.|Puede haber varias.|

### Foreign key

````python
# Garden has a one-to-many relationship with Flower
class Gardener(models.Model):
  first_name = models.CharField(max_length=20)
  years_experience = models.IntegerField()

# Flower has a many-to-one relationship with Gardener
class Flower(models.Model):
  petal_color = models.CharField(max_length=10)
  petal_number = models.IntegerField()
  gardener = models.ForeignKey(Gardener, on_delete=models.CASCADE) 
````

Notice that we added the gardener field using models.ForeignKey() and with arguments. The first argument is Gardener because that’s the model we want this foreign key to come from. Then we add on_delete=models.CASCADE to let Django know to delete the Flower instance if its connected Gardener instance is deleted. 

models.CASCADE: When the referenced object is deleted, all objects referencing it (child objects) are also deleted.
Example: If a User is deleted, all their Posts are also removed.

## Field Type Optional Arguments

One common option is null that can take on a value of True or False. This null option tells the database if a field can be left intentionally void of information. By default, Django sets null=False. However, we can override the default and set null=True. Here’s an example:

````python
class Flower(model.Model):
  petal_number = models.IntegerField(max_length=20, null=True)
  # Other fields 
````

Another common option is blank, which is similar to null, but setting blank to True means a field doesn’t have to take anything, not even a null value. By default blank is False.

````python
class Flower(model.Model):
  nickname = models.CharField(max_length=20, blank=True)
  # Other fields
````

The last one we’ll touch upon is choices which limits the input a field can accept. We can set choices by creating a list of tuples that contain 2 items: a key and a value. Take for example:

````python
class Flower(models.Model):
  COLOR_CHOICES = [
     ("R", "Red"),
     ("Y", "Yellow"),
     ("P", "Purple"),
     ("O", "Other"),
  ]
  
  color = models.CharField(max_length=1, choices=COLOR_CHOICES)
  # Other fields
````

For our Flower instance, we can specify its color with the limited choices provided — meaning our color field can only be assigned "R" (for "Red"), "Y" (for "Yellow"), or "P" (for "Purple"), or "O" (for "Other" from the COLOR_CHOICES list. With choices we know exactly what data we can accept from users.

**models.py**

````python
from django.db import models

class Owner(models.Model):
  first_name = models.CharField(max_length=30)
  last_name = models.CharField(max_length=30)
  phone = models.CharField(max_length=30)

class Patient(models.Model):
  DOG= "DO"
  CAT= "CA"
  BIRD= "BI"
  REPTILE= "RE"
  OTHER= "OT"
  
  ANIMAL_TYPE_CHOICES= [
    (DOG, "Dog"),
    (CAT, "Cat"),
    (BIRD, "Bird"),
    (REPTILE, "Reptile"),
    (OTHER, "Other"),
  ]

  animal_type= models.CharField(max_length=2, choices= ANIMAL_TYPE_CHOICES, default=OTHER)
  breed = models.CharField(max_length=200)
  pet_name = models.CharField(max_length=200)
  age = models.IntegerField(default=0)
  owner = models.ForeignKey(Owner, on_delete=models.CASCADE)
````

## Metadata

https://docs.djangoproject.com/en/3.1/ref/models/options/#model-meta-options

````python
class Flower(models.Model):
  name = models.CharField(max_length=10)
  # All the other attributes
  
  class Meta:
    ordering = ["name"]


In this case, we created an attribute, ordering which takes a list of strings (["name"]) that determine the ordering. Later on, when we need to search for Flower instances, the database will return back a list with the list ordered by the name field. We can even reverse the order by adding a - in front of a string like ["-name"].

````python
from django.db import models

class Owner(models.Model):
  first_name = models.CharField(max_length=30)
  last_name = models.CharField(max_length=30)
  phone = models.CharField(max_length=30)

class Patient(models.Model):
  DOG = "DO"
  CAT = "CA"
  BIRD = "BI"
  REPTILE = "RE"
  OTHER = "OT"
  ANIMAL_TYPE_CHOICES = [
    (DOG, "Dog"),
    (CAT, "Cat"),
    (BIRD, "Bird"),
    (REPTILE, "Reptile"),
    (OTHER, "Other"),
  ]
  class Meta:
    ordering= ["pet_name"]
  animal_type = models.CharField(max_length=2, choices=ANIMAL_TYPE_CHOICES, default=OTHER)
  breed = models.CharField(max_length=200)
  pet_name = models.CharField(max_length=200)
  age = models.IntegerField(default=0)
  owner = models.ForeignKey(Owner, on_delete=models.CASCADE)
````

## Native Model Methods

The properties we’ve created for our flowers describe what our flower is or has. They are like the nouns and adjectives that describe each flower. What we are missing though, and why modeling database data is so useful to begin with, are the verbs, the actions associated with our flowers. These are called methods. Methods are functions defined in our model that describe the behaviors and actions of our model. If properties are what our models are, then methods are what our models do. For example, our flower might bloom() and grow().
In Python classes, which Django uses to create models, there are built-in methods we can override like the /__str__ method. All this means is we are creating a method using the same name as the built-in one. This is how we, the programmer, take control, or “override”, the default behavior of the built-in version:

````python
class Gardener(models.Model):
  name = models.CharField(max_length=30)
  
  def __str__(self):
    return self.name
````

Methods always require the first 
parameter
Preview: Docs Loading link description
 to be self, then we can provide other optional parameters and add logic within the method body.

## Custom Model Methods

In addition to overriding native methods, we can define our own custom methods!

We can do something simple like returning a boolean:

````python
class Flower(models.Model):
  has_sunlight = models.BooleanField(default=True)
  has_water = models.BooleanField(default=True)

  def can_grow(self):
    return self.has_sunlight and self.has_water
````

---

````python
class Gardener(models.Model):
  years_experience = models.IntegerField()

  def identify_flower(self, flower):
    return f"I've studied flowers for {self.years_experience}. I believe this flower is {flower.name} and is found in {flower.native_env}." 
````

## Migrations - makemigrations

migrations are needed when we make changes to our models

In Django, there are two steps necessary to make this migration happen:

- Running **python3 manage.py makemigrations** to create a file with the instructions needed for our database to create the proper schemas. **In root folder.**
- Running **python3 manage.py migrate** to execute the instructions in our file to create the actual tables in our database.

If we just want to create the models of one app insted of all of them. We can use:

**python3 manage.py makemigrations garden.**

The files created from this step live in the migrations folder within our app directory. Our first migration file would begin with **0001_initial.py**. We can refer to our migrations using the starting numbers, in this case, it has a migration name of 0001.

It’s important that every time we need to make a change to the schema in our database we need to do this makemigrations step! Subsequent migration files will increase the number at the beginning of the file. For example, the second migration will begin with 0002_xxxxx.py and so forth.

If we need to reverse a migration, Django also makes this possible by specifying the migration we want to revert back to:

**python3 manage.py migrate \<app_name> \<migration_name>**


The \<migration_name> would be something like 0001 or 0002 etc., depending on which migration we are reverting back to. We can use the command showmigrations to see a list of all the migrations.

**python3 manage.py showmigrations garden**


- A schema is a structure we design for the data in our application.
- A model is a Python class that contains characteristics and behavior using: attributes, metadata, and methods.
- Model attributes are implemented using Django field names and different field types.
- Django models can relate to other models. One way of showing this connection is to use a foreign key.
- Django field types accept optional keyword arguments based on key-value pairs such as null, blank, choices, default, and primary_key.
- Models can contain metadata using a nested Meta class and providing specific attributes.
- Models can have methods that are functions specific to that model. Some methods are inherited from the parent Model class.
- Django requires that we commit our models to the database via a two-step migration procedure with makemigrations and migrate.
- Django lets us undo one or more migrations by supplying the migrate command with a named migration.
























































