# CRUD Django Example

#### An example of how to use the Django Framework to create a CRUD application of Books in MySQL.

## Start a new Django Project

1. Install Django
```bash
pip install django
```

2. Create a new project. Replace "myproject" with your project name. It's common to use uppercase letters at the start of each word, such as "BookStore"
```bash
python -m django startproject myproject
```

3. Enter the project folder
```bash
cd myproject
```

4. Install Virtualenv. Virtualenv allows you to create isolated Python environments. This is especially useful for managing dependencies and avoiding conflicts between packages used in different projects.
```bash
pip install virtualenv
```

5. Create a Virtual Environment. Replace "myenv" with the name of your virtual environment. I usually use the name "venv"
```bash
python3 -m venv myenv
```

6. Activate the Virtual Environment. This depends on your operating system.
- Windows:
```bash
venv\Scripts\activate
```
- Linux:
```bash
source venv/bin/activate
```

7. Now you can install dependencies, and they will be isolated. To use it, just install a dependency.
```bash
pip install django
```
Then you must run:
```bash
pip freeze > requirements.txt
```
With this, you can store all the project dependencies in this file, which is useful for Docker and server deployments.

8. Use deactivate to exit the Virtual Environment.
```bash
deactivate
```

9. Run the project by executing:
```bash
python manage.py runserver
```
- The basic project will be available at http://127.0.0.1:8000/

<img src="https://github.com/GDS2005/django-crud/blob/main/assets/django-basic.jpg" alt="django-basic Image" width="600"/>

## Generate CRUD (Create, Read, Update and Delete) of elements

1. First, we need to create an app. A Django app is a modular component representing a discrete part of a larger project. The app name should be short, all-lowercase, and not include numbers, dashes, periods, spaces, or special characters. It should generally be the plural form of an app's main model.
```bash
python3 manage.py startapp appname
```

2. Add the new app to the config/settings.py
```bash
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'appname', <--
]
```

3. Define the model. In appname/models.py, define the model. For example:
```bash
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=100)
    description = models.TextField()
    price = models.DecimalField(max_digits=10, decimal_places=2)

    class Meta:
        db_table = 'books' 

        def __str__(self):
            return self.id
```

4. Create forms to manage the CRUD. In appname, create a file named forms.py for the Book model
```bash
from django import forms
from .models import Book

class BookForm(forms.ModelForm):
    class Meta:
        model = Book
        fields = ['title', 'author', 'description', 'price']
```

5. In appname/views.py, create functions for the CRUD operations, for example:
```bash
from django.shortcuts import render, get_object_or_404, redirect
from .models import Book
from .forms import BookForm

def book_list(request):
    books = Product.objects.all()
    return render(request, 'books/book_list.html', {'books': books})
```

6. Define the URL patterns in a urls.py file within your app:
```bash
from django.urls import path
from .views import book_list

urlpatterns = [
    path('', book_list, name='book_list'),
]   
```

7. Include the app's URLs in the main project. In myproject/urls.py, add:
```bash
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('books.urls')),
]
```

8. Create the template to add HTML content. Create a folder named templates in the app's directory.

## Configure Database

#### I highly recommend using Decouple to use environment variables.

```bash
pip install python-decouple
```
Then, create a ".env" file and define your variables:

```bash
DB_HOST=localhost
DB_PORT=3306
DB_NAME=my_database
DB_USER=my_user
DB_PASSWORD=my_password
```

Import the Decouple package wherever you need it.

```bash
from decouple import config
```

1. Install te database.
- Mysql
```bash
pip install mysqlclient
```

- MongoDB
```bash
pip install djongo
```

- Postgress
```bash
pip install psycopg2
```

2. Depending on our database, we need to modify the DATABASE configuration in myproject/config.py:

- Mysql:
```bash
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": config('DB_NAME'),
        "USER": config('DB_USER'),
        "PASSWORD": config('DB_PASSWORD'),
        "HOST": config('DB_HOST'),
        "PORT": config('DB_PORT'),
    }
}

```
- Sqlite:
```bash
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
- MongoDB:
```bash
DATABASES = {
    'default': {
        'ENGINE' : 'djongo',
        'NAME' : config('DB_NAME'),
        'HOST': config('DB_HOST'),
        'PORT': config('DB_PORT'),
        'USERNAME': config('DB_USER'),
        'PASSWORD': config('DB_PASSWORD'),
        'MONGO_URI': "mongodb://admin:mypwd@127.0.0.127017/bookstorage"
    }
}
```
- Postgres:
```bash
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": config('DB_NAME'),
        "USER": config('DB_USER'),
        "PASSWORD": config('DB_PASSWORD'),
        "HOST": config('DB_HOST'),
        "PORT": config('DB_PORT'),
    }
}
```

3. Make a migration to apply changes to the database. MAKE SURE THE DATABASE IS CREATED.
```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## How to clone and practice with this repository

1. Clone this repository.
```bash
git clone https://github.com/yourusername/your-django-project.git
```

2. Install dependencies.
```bash
pip install -r requirements.txt
```

3. Create the .env file and define the following variables:
```bash
DB_HOST=localhost
DB_PORT=3306
DB_NAME=my_database
DB_USER=my_user
DB_PASSWORD=my_password
```

4. Create the database. 

5. Run migrations to apply changes to the database.
```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

6. Run the project:
```bash
python manage.py runserver
```

## Contributing
Contributions are welcome!

