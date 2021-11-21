# Django

{% embed url="https://docs.djangoproject.com/en/3.2/intro/tutorial01/" %}

Project directory:

```
app/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

`app` is the container for the project

`manage.py` is the command line utility to manage your project

`mysite` is the actual package. Its name is the name for imports, eg: `mysite.urls`

`mysite/__init__.py` empty file indicating the directory is a python package [https://docs.python.org/3/tutorial/modules.html#tut-packages](https://docs.python.org/3/tutorial/modules.html#tut-packages)

`mysite/settings.py` is the config

`urls.py` url declarations

`asgi.py` entry point for asgi-compatible web servers

`wsgi.py` entry point for wsgi-compatible web servers



To run server:

```
$ python manage.py runserver
```

### View

{% code title="views.py" %}
```
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```
{% endcode %}

### Urls

{% code title="polls/urls.py" %}
```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```
{% endcode %}



{% code title="mysite/urls.py" %}
```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```
{% endcode %}

The [`include()`](https://docs.djangoproject.com/en/3.2/ref/urls/#django.urls.include) function allows referencing other URLconfs. Whenever Django encounters [`include()`](https://docs.djangoproject.com/en/3.2/ref/urls/#django.urls.include), it chops off whatever part of the URL matched up to that point and sends the remaining string to the included URLconf for further processing.

The idea behind [`include()`](https://docs.djangoproject.com/en/3.2/ref/urls/#django.urls.include) is to make it easy to plug-and-play URLs. Since polls are in their own URLconf (`polls/urls.py`), they can be placed under “/polls/”, or under “/fun\_polls/”, or under “/content/polls/”, or any other path root, and the app will still work.

The [`path()`](https://docs.djangoproject.com/en/3.2/ref/urls/#django.urls.path) function is passed four arguments, two required: `route` and `view`, and two optional: `kwargs`, and `name`.
