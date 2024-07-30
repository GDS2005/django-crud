# CRUD Django Example

#### An example of how to use Django Framework and make a CRUD of Books in Mysql.

## Start a new Django Project

1. Install Django
```bash
pip install django
```

2. Create a new project. Remplece "myproject" with your project name. Usually use Uppercase at the start of every string like "BookStore"
```bash
python -m django startproject myproject
```

3. Enter to the folder project
```bash
cd myproject
```

4. Install Virtualenv. Virtualenv allows you to create isolated Python environments. This is particularly useful for managing dependencies and avoiding conflicts between packages used in different projects.
```bash
pip install virtualenv
```

5. Create a Virtual Enviroment. Change "myenv" for the name of your Virtual Enviroment; I usually use the name "venv".
```bash
python3 -m venv myenv
```

6. Activate the Virtual Enviroment. This depends of your Operative System.
- Windows:
```bash
venv\Scripts\activate
```
- Linux:
```bash
source venv/bin/activate
```

7. Now you can install dependencies and it will be isolated. To use it just install a dependencie.
```bash
pip install django
```
then you must use:
```bash
pip freeze > requirements.txt
```

With this, you will be able to storage all the dependencies of the project in this file that it is usefull in Docker and servers deploys. 
8. Use deactivate to end the Virtual Enviroment.
```bash
deactivate
```

9. Run the project with:
```bash
python manage.py runserver
```
- The basic project will start at http://127.0.0.1:8000/

## Generate CRUD (Create, Read, Update and Delete) of elements

1. First we need to create an app. A Django app is a small library representing a discrete part of a larger project. The app name should be short, all-lowercase, and not include numbers, dashes, periods, spaces, or special characters. It also, in general, should be the plural of an app's main model.
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

3. Define the Model. In `appname/models.py`, define the model. For example:
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

4. Create Forms to manage the CRUD. In `appname`generate a file named forms for the Product model:
```bash
from django import forms
from .models import Book

class BookForm(forms.ModelForm):
    class Meta:
        model = Book
        fields = ['title', 'author', 'description', 'price']
```

5. In `appname/views.py`, create functions for the CRUD operation, for example:
```bash
from django.shortcuts import render, get_object_or_404, redirect
from .models import Book
from .forms import BookForm

def book_list(request):
    books = Product.objects.all()
    return render(request, 'books/book_list.html', {'books': books})
```

6. Create the urls.py file in your app to navigate to define the URL patterns:
```bash
from django.urls import path
from .views import book_list

urlpatterns = [
    path('', book_list, name='book_list'),
]   
```

7. Include URLs in the main Project. In `myproject/urls.py`, include the app's URLs:
```bash
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('books.urls')),
]
```

8. Create the template to add html. Create a folder named templates in appname.

## Configure Database

#### I highly recommend to use Decouple to use Enviroment Variables.

```bash
pip install python-decouple
```
Then, generate a ".env" file and create your variables:

```bash
DB_HOST=localhost
DB_PORT=3306
DB_NAME=my_database
DB_USER=my_user
DB_PASSWORD=my_password
```

Import the decouple dependencie where ever you need it.

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

2. Depending of our database, we need to modify the DATABASE configuration in myproject/config.py:

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

3. Make a migration to generate the database. MAKE SURE THAT THE DATABASE IS CREATED.
```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## To clone an practice with this repository

1. Clone the repository.
```bash
git clone https://github.com/yourusername/your-django-project.git
```

2. Install dependencies.
```bash
pip install -r requirements.txt
```

3. Generate the .env file with:
```bash
DB_HOST=localhost
DB_PORT=3306
DB_NAME=my_database
DB_USER=my_user
DB_PASSWORD=my_password
```

4. Create the database. 

5. Make a migration to generate the database. 
```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

6. Run the project:
```bash
python manage.py runserver
```