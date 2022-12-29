# [Recipe-API][docs]

**An advanced recipe API that allows you to upload and store some of your favourite `recipes` from photos and the web.**


It is a simple [Django](https://www.djangoproject.com/) project that uses [Django REST framework](http://www.django-rest-framework.org/). The template makes use of Docker to create several containers, with
specific tasks aimed to development or to productionize the project.
<img width="1673" alt="Django1" src="https://user-images.githubusercontent.com/35616113/209976569-4a2b2622-6e4a-407f-9a24-e6d7012046fe.png">

---


# Installation Overview

Django REST framework is a powerful and flexible toolkit for building Web APIs.

Some reasons you might want to use REST framework:

* The [Web browsable API][sandbox] is a huge usability win for your developers.
* [Authentication policies][authentication] including optional packages for [OAuth1a][oauth1-section] and [OAuth2][oauth2-section].
* [Serialization][serializers] that supports both [ORM][modelserializer-section] and [non-ORM][serializer-section] data sources.
* Customizable all the way down - just use [regular function-based views][functionview-section] if you don't need the [more][generic-views] [powerful][viewsets] [features][routers].
* [Extensive documentation][docs], and [great community support][group].


----

# Requirements

* Python 3.9+
* Django 4.1, 4.0, 3.2, 3.1, 3.0

We **highly recommend** and only officially support the latest patch release of
each Python and Django series.

# Installation

Install using `pip`...

    pip install djangorestframework

Add `'rest_framework'` to your `INSTALLED_APPS` setting.
```python
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

# Example

Let's take a look at a quick example of using REST framework to build a simple model-backed API for accessing users and groups.

Startup up a new project like so...

    pip install django
    pip install djangorestframework
    django-admin startproject example .
    ./manage.py migrate
    ./manage.py createsuperuser


Now edit the `example/urls.py` module in your project:

```python
from django.contrib.auth.models import User
from django.urls import include, path
from rest_framework import routers, serializers, viewsets


# Serializers define the API representation.
class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ['url', 'username', 'email', 'is_staff']


# ViewSets define the view behavior.
class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer


# Routers provide a way of automatically determining the URL conf.
router = routers.DefaultRouter()
router.register(r'users', UserViewSet)

# Wire up our API using automatic URL routing.
# Additionally, we include login URLs for the browsable API.
urlpatterns = [
    path('', include(router.urls)),
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework')),
]
```

We'd also like to configure a couple of settings for our API.

Add the following to your `settings.py` module:

```python
INSTALLED_APPS = [
    ...  # Make sure to include the default installed apps here.
    'rest_framework',
]

REST_FRAMEWORK = {
    # Use Django's standard `django.contrib.auth` permissions,
    # or allow read-only access for unauthenticated users.
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.DjangoModelPermissionsOrAnonReadOnly',
    ]
}
```

That's it, we're done!

    ./manage.py runserver

You can now open the API in your browser at `http://127.0.0.1:8000/`, and view your new 'users' API. If you use the `Login` control in the top right corner you'll also be able to add, create and delete users from the system.

You can also interact with the API using command line tools such as [`curl`](https://curl.haxx.se/). For example, to list the users endpoint:

    $ curl -H 'Accept: application/json; indent=4' -u admin:password http://127.0.0.1:8000/users/
    [
        {
            "url": "http://127.0.0.1:8000/users/1/",
            "username": "admin",
            "email": "admin@example.com",
            "is_staff": true,
        }
    ]

Or to create a new user:

    $ curl -X POST -d username=new -d email=new@example.com -d is_staff=false -H 'Accept: application/json; indent=4' -u admin:password http://127.0.0.1:8000/users/
    {
        "url": "http://127.0.0.1:8000/users/2/",
        "username": "new",
        "email": "new@example.com",
        "is_staff": false,
    }

# Documentation & Support

Full documentation for the project is available at [https://www.django-rest-framework.org/][docs].

For questions and support, use the [REST framework discussion group][group], or `#restframework` on libera.chat IRC.

You may also want to [follow the author on Twitter][twitter].

# Security

Please see the [security policy][security-policy].

[build-status-image]: https://github.com/encode/django-rest-framework/actions/workflows/main.yml/badge.svg
[build-status]: https://github.com/encode/django-rest-framework/actions/workflows/main.yml
[coverage-status-image]: https://img.shields.io/codecov/c/github/encode/django-rest-framework/master.svg
[codecov]: https://codecov.io/github/encode/django-rest-framework?branch=master
[pypi-version]: https://img.shields.io/pypi/v/djangorestframework.svg
[pypi]: https://pypi.org/project/djangorestframework/
[twitter]: https://twitter.com/starletdreaming
[group]: https://groups.google.com/forum/?fromgroups#!forum/django-rest-framework
[sandbox]: https://restframework.herokuapp.com/

[funding]: https://fund.django-rest-framework.org/topics/funding/
[sponsors]: https://fund.django-rest-framework.org/topics/funding/#our-sponsors

[sentry-img]: https://raw.githubusercontent.com/encode/django-rest-framework/master/docs/img/premium/sentry-readme.png
[stream-img]: https://raw.githubusercontent.com/encode/django-rest-framework/master/docs/img/premium/stream-readme.png
[spacinov-img]: https://raw.githubusercontent.com/encode/django-rest-framework/master/docs/img/premium/spacinov-readme.png
[retool-img]: https://raw.githubusercontent.com/encode/django-rest-framework/master/docs/img/premium/retool-readme.png
[bitio-img]: https://raw.githubusercontent.com/encode/django-rest-framework/master/docs/img/premium/bitio-readme.png
[posthog-img]: https://raw.githubusercontent.com/encode/django-rest-framework/master/docs/img/premium/posthog-readme.png
[cryptapi-img]: https://raw.githubusercontent.com/encode/django-rest-framework/master/docs/img/premium/cryptapi-readme.png
[fezto-img]: https://raw.githubusercontent.com/encode/django-rest-framework/master/docs/img/premium/fezto-readme.png

[sentry-url]: https://getsentry.com/welcome/
[stream-url]: https://getstream.io/?utm_source=DjangoRESTFramework&utm_medium=Webpage_Logo_Ad&utm_content=Developer&utm_campaign=DjangoRESTFramework_Jan2022_HomePage
[spacinov-url]: https://www.spacinov.com/
[retool-url]: https://retool.com/?utm_source=djangorest&utm_medium=sponsorship
[bitio-url]: https://bit.io/jobs?utm_source=DRF&utm_medium=sponsor&utm_campaign=DRF_sponsorship
[posthog-url]: https://posthog.com?utm_source=drf&utm_medium=sponsorship&utm_campaign=open-source-sponsorship
[cryptapi-url]: https://cryptapi.io
[fezto-url]: https://www.fezto.xyz/?utm_source=DjangoRESTFramework

[oauth1-section]: https://www.django-rest-framework.org/api-guide/authentication/#django-rest-framework-oauth
[oauth2-section]: https://www.django-rest-framework.org/api-guide/authentication/#django-oauth-toolkit
[serializer-section]: https://www.django-rest-framework.org/api-guide/serializers/#serializers
[modelserializer-section]: https://www.django-rest-framework.org/api-guide/serializers/#modelserializer
[functionview-section]: https://www.django-rest-framework.org/api-guide/views/#function-based-views
[generic-views]: https://www.django-rest-framework.org/api-guide/generic-views/
[viewsets]: https://www.django-rest-framework.org/api-guide/viewsets/
[routers]: https://www.django-rest-framework.org/api-guide/routers/
[serializers]: https://www.django-rest-framework.org/api-guide/serializers/
[authentication]: https://www.django-rest-framework.org/api-guide/authentication/
[image]: https://www.django-rest-framework.org/img/quickstart.png

[docs]: https://www.django-rest-framework.org/
[security-policy]: https://github.com/encode/django-rest-framework/security/policy

Docker services for development
=========

The main docker-compose file has all the services to run at development

- *test*: Run the using tests, using [pytest](https://docs.pytest.org) and 
          [pytest-django](https://pytest-django.readthedocs.io/)

```
    docker-compose run test [pytest args]
```
  pytest is very powerful and allows a big variety of parameters, I recomend that everyone checks the docs and
learn a little bit about it. Some examples

```
    # Run all tests
    docker-compose run test
    # Recreate the DB
    docker-compose run test --create-db
    # Run tests that fits with stringexp
    docker-compose run test -k stringexp
    # Run failed tests from last run
    docker-compose run test --lf
``` 

  Some basic tests are added to the template. Note that the logs are directed to the console while running
the tests, and will be captured by pytest.


- *dev-server*: Run a dev server, aimed to check interactivly the app through a browser. It
                can be accessed at port 8000, and it will restart if the code of the application 
                changes. It is using the django `runserver` command under the hood.
```
docker-compose up [-d] dev-server
```
- *db*: Database backend. It is started containing the data in the fixtures described in the code.


  Most of the changes in the code doesn't require restarting the services or rebuilding, but changes
in the DB do, like new fixtures or a new migration. Build all services with

```
    docker-compose build
```

- *build-deps*: Precompile all dependencies into wheels and copy them in ./vendor. This is not
required, but can save time when rebuilding the containers often, to avoid compile the same code
over and over. Check the more detailed documentation in the ./vendor/README.md file.
  Note that dependencies embedded in ./deps won't be compiled (though their dependencies will be).
Check more details in the ./deps/README.md file. 

- *system-test*: Run system tests over the whole system. It send requests to the started system
and checks the results. It used pytest, so all the details from *test* works here.
  Remember to be sure to rebuild the *server* service before running them. Be also sure to check the
logs from the *log* service for insight while running them.

- *log*: A syslog facility that centralises all the logs. All the system will direct their logs 
to here, making convenient to check. You can check on the logs as they are generated running

```
docker-compose exec log tail -f /var/log/templatesite.log
```
  Remember that restarting the container will clean the file. This is convenient to not keep old logs
around, but it needs to keep in mind. Generallt, you don not need to bring down this container.

  Most important logs are the one generated by Django, that are prepend with "templatesite". A 
request id is added on each request helping group logs. This request id can be supplied externally
using the header X-REQUEST-ID, and it will be returned with the response.

- *metrics*: Report metrics in a [Prometheus](https://prometheus.io/) format. The Prometheus console 
can be checked in the port 9090. The metrics are exported in the server in the url /metrics

  A Django dashboard can be found at `http://localhost:9090/consoles/django.html`. This can be 
tweaked in the file ./docker/metrics/consoles/django.html

- *metrics-graph*: A Grafana container, as reference. This is presented directly from the Grafana 
standard container, and it should be pointed towards the metrics container 
in http://metrics:9090/. Follow the instructions in 
the [Grafana docs](http://docs.grafana.org/installation/docker/)
  Graphs and dashboards may be added, for example, querying:
```
    rate(django_http_requests_total_by_view_transport_method[1m])
```
To display all Django views. Be careful as the inherent non persistency of containers may destroy
your dashboards. This should be used only as example. Getting good dashboards is important for 
production, but not so much for development.
