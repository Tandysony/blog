---
layout: post
title: Django MVC
tags: [Django, Python]
---

## 1. Django Overview

**Django is HTTP in, and HTTP out**, as shown in the figure below. Django is the stuff in yellow. The `URLs` tell the `Request` where it should go to pick up something. The `VIEWS` are python callables that return response. Within the `VIEWS` of a very basic web application, you might end up using MODELS and TEMPLATES only. The VIEWS will use MODELS to talk to database, and the TEMPLATES to render some HTMLs.

![HTTP in HTTP out](../../../assets/img/thumbnails/django-http-in-out.png)

Taken together, these pieces loosely follow a pattern called `Model-View-Controller (MVC)`. MVC is a way of developing software so that the code for defining and accessing data (the `Model`) is separate from request-routing logic (the `Controller`), which in turn is separate from the user interface (the `View`). Django framework follows the MVC pattern, but uses **non-idiomatic** ways of naming the parts of the MVC triad. The mapping between Django terms and the idiomatic terminology is shown in the table below:

| Idiomatic term | Django term | Meaning                                                                                          |
| -------------- | ----------- | ------------------------------------------------------------------------------------------------ |
| Model          | Model       | Contains all the business logic. At the very least the database access logic.                    |
| View           | Template    | Responsible for generating the HTML and other UI elements.                                       |
| Controller     | View        | Contains the logic to tie the other parts together and to generate a response to a user request. |

- Models in Django

  [https://docs.djangoproject.com/en/1.11/topics/db/models/](https://docs.djangoproject.com/en/1.11/topics/db/models/)

- Views in Django]

  [https://docs.djangoproject.com/en/1.11/topics/class-based-views/](https://docs.djangoproject.com/en/1.11/topics/http/views/)

  [https://docs.djangoproject.com/en/1.11/topics/class-based-views/](https://docs.djangoproject.com/en/1.11/topics/class-based-views/)

- Templates in Django

  [https://docs.djangoproject.com/en/1.11/topics/templates/](https://docs.djangoproject.com/en/1.11/topics/templates/)

## 2. Starting A Django Project

### 2.1. Create a Django project

There is a handy tool to establish a project in command line:

```bash
$ django-admin startproject webapp
```

The webapp project will be created in a tree-view structure:

```
webapp/
    manage.py
    webapp/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

And these files are:

- The outer `webapp/` root directory is just a container for your project. Its name doesn’t matter to Django; you can rename it to anything you like.

- `manage.py`: A command-line utility (command runner) that lets you interact with this Django project in various ways. You can read all the details about `manage.py` in [`django-admin` and `manage.py`](https://docs.djangoproject.com/en/1.11/ref/django-admin/).

- The inner `webapp/` directory is the actual Python package for your project. Its name is the Python package name you’ll need to use to import anything inside it (e.g. `webapp.urls`).

- `webapp/__init__.py`: An empty file that tells Python that this directory should be considered a Python package. (Read more about [packages](https://docs.python.org/3/tutorial/modules.html#tut-packages) in the official Python docs if you’re a Python beginner.)

- `webapp/settings.py`: Settings/configuration for this Django project. Django [settings](https://docs.djangoproject.com/en/1.11/topics/settings/) will tell you all about how settings work.

- `webapp/urls.py`: The URL declarations for this Django project; a “table of contents” of your Django-powered site. You can read more about URLs in [URL dispatcher](https://docs.djangoproject.com/en/1.11/topics/http/urls/).

- `webapp/wsgi.py`: An entry-point for WSGI-compatible web servers to serve your project. See [How to deploy with WSGI](https://docs.djangoproject.com/en/1.11/howto/deployment/wsgi/) for more details.

### 2.2. Start a python server

To start a python server, you need to run the following command in the command line: `python manage.py runserver` , and you will be seeming the following information:

```bash
Validating models…
0 errors found
January 5, 2018 - 15:50:53
Django version 1.11.9, using settings 'webapp.settings'
Development server is running at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

This indicates that you have successfully started a server for your Django project. Go to http://127.0.0.1:8000/, you should be seeing a welcome page.
