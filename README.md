# django-timelog

Django Timelog App. Timesheet time tracking for a person or group of people.

An alternative to my [saas-by-erik/timelog](https://github.com/saas-by-erik/timelog).

Made with great help from https://docs.djangoproject.com/en/1.7/intro/tutorial01/

Currently a work in progress.

## Compatibility

Compatible with Django 1.7+. Requires Python 3.

## Setup

Describing the setup procedure using Debian GNU/Linux 7.

```
# apt-get install postgresql libpq-dev python3-dateutil python3-pip
# pip-3.2 install django psycopg2
# adduser timelog
# su - postgresql
$ psql
```

```
CREATE USER timelog;
CREATE DATABASE timelog OWNER timelog;
\q
```

```
$ exit
# su - timelog
$ django-admin.py startproject serve
$ cd serve/
$ git clone https://github.com/erikano/django-timelog.git timelog/
$ patch -p2 -d serve/ < timelog/patch/serve/settings.py.patch
$ patch -p2 -d serve/ < timelog/patch/serve/urls.py.patch
$ export EDITOR=vim # Set it to your prefered editor.
$ $EDITOR serve/settings.py # Edit TIME_ZONE.
$ python3 manage.py makemigrations timelog
$ python3 manage.py migrate
$ python3 manage.py createsuperuser # it will suggest using name 'timelog'. Let it.
$ python3 manage.py runserver 0.0.0.0:8000 &
```

## Usage from command-line

Since the web UI has not yet been created, here is some initial basic usage.

We are not yet dealing with other users.

```
$ python3 ~/serve/manage.py shell
```

```
from timelog.models import Category, Entry
c = Category(name='Example')
c.save()
from django.contrib.auth.models import User
u = User.objects.get(username='timelog')
from django.utils import timezone
e = Entry(user=u, category=c, t_begin=timezone.now())
e.save()
exit()
```

## Usage with default Django admin web interface

You *could* begin entering data into django-timelog right now at
`http://<host or IP>:8000/admin/timelog/`.
It's not great but it's better than nothing
and better than using it through the shell like above.

## Updating

```
$ cd ~/serve/timelog \
  && git pull \
  && python3 ../manage.py makemigrations timelog \
  && python3 ../manage.py migrate
```
