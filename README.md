# django-tricks
> This repository is created to save you from some of the common problems that occur during the development of a django app.

* [When changing ```DEBUG-=-True```-to-```DEBUG-=-False``` in settings.py file](#When-changing-```DEBUG-=-True```-to-```DEBUG-=-False```-in-settings.py-file) 
## When changing ```DEBUG = True``` to ```DEBUG = False``` in settings.py file 
  First things first. **DEBUG** allows us to see the errors and traceback them. Now if our app raise an exception, django shows a detailed **trackeback** which includes "almost" every information related to our django environmnet and currently defined settings in settings.py file, except some sensitive information. With **DEBUG** turned on, the developer can see the reason of the error and remove them. It's only good during development, not production. 
When you change the settings from ```DEBUG = True``` to ```DEBUG = False``` in settings.py file, there are things to be taken care of which are:
  * You have to set **ALLOWED_HOSTS** setting, so that your app don't return **"Bad Request (400)"** error on requesting. For example:
  ```python
    ALLOWED_HOSTS = [
        'example.com',
    ]
```
  * Django doesn't take care of your static files with DEBUG turned off, but it allows the web server you chose for production, to take care of the static files for you. See [here](https://docs.djangoproject.com/en/1.11/ref/settings/#debug)
  * If you still want to load the static files with DEBUG turned off (```DEBUG = False```), run the following command to run the development server in insecure mode:
  ```console
  python manage.py runserver --insecure
  ```
  #### Warning - Never ever deploy a site or a django app into production with DEBUG turned on or ```DEBUG =True```.
