---
layout: post
title: Setting Up MySQL Database and Email for a Django Project
tags: [Django, Python]
---

## 1. Set up MySQL for a Django Project

`SQLite` is the default database engine in Django. Here, I am using `MySQL` for local development.

- Create a database to be used

How to create a MySQL database is beyond the scope of this blog. I have introduced in the blog ["Deploy a Django App to AWS"](../09/deploy-a-django-app-to-aws.html). I use [AMPPS stack](http://www.ampps.com/) to configure Apache and MySQL environment, and use phpMyAdmin to `new` a database named “django”, for local development.

![Create a database](../../../assets/img/thumbnails/new-database.png)

- Configure your application to the database

  > This is for local development and personal project only. If you are developing a commercial web application within a team, please use the package [`python-decouple`](https://github.com/henriquebastos/python-decouple) to protect your sensitive information, like `database name`, `database user`, `database password`, and `database host`.

Open the `webapp/settings.py` file, and configure the required database parameters

```bash
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django',
        'USER': 'root',
        'PASSWORD': 'mysql',
        'HOST': '',
        'PORT': '3306',
    }
}
```

Please refer to the [official document on database settings](https://docs.djangoproject.com/en/1.11/ref/settings/#std:setting-DATABASES), for more detailed setting explanations. Please do not use such a simply username and password pair on a production server.

## 2. Use GMail as a free SMTP server

- Add following into the `settings.py` (or, `settings_local.py`, depending on your project setups) file:

> Again, the sensitive information can be protected using the aforementioned `python-decouple` package.

```bash
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_USE_TLS = True
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_HOST_USER = 'test@gmail.com'
EMAIL_HOST_PASSWORD = 'test'
EMAIL_PORT = 587
```

- And in the `views.py` file

```bash
from django.core.mail import EmailMessage

email = EmailMessage('title', 'body', to=[email])
email.send()
```

- Further more, remember to unlock Captcha over your Gmail account at :

[https://accounts.google.com/DisplayUnlockCaptcha](https://accounts.google.com/DisplayUnlockCaptcha)

Also go to [https://myaccount.google.com/security#connectedapps](https://myaccount.google.com/security#connectedapps) to enable `Allow less secure apps`. If not, you will be encountering `SMTPAuthenticationError`.

For transitional Email settings using a 3-rd party service, e.g. AWS SES(Simple Email Service), I will write a blog later.
