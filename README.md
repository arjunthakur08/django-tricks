# Django Trickster :bulb:
> This repository is created to save you from some of the common problems that occur during the development of a django app.

* [Creating Virtual Environment](#creating-virtual-environment)
* [Setting Up the PostgreSQL DB locally](#setting-up-the-postgresql-db)
* [When changing ```DEBUG = True```to ```DEBUG = False``` in settings.py file](#when-changing-debug--true-to-debug--false-in-settingspy-file)
* [Handling Class-based view of django authentication system \(New in django 1.11+\)](#handling-class-based-view-of-djangos-authentication-system-new-in-django-111)

# Bonus Tricks :gift:
> Some of the hacks you might not know

* [Automatically redirecting from HTTP to HTTPS](#automatically-redirecting-from-http-to-https)
* [Force redirecting URLs](#force-redirecting-urls)

# Creating Virtual Environment

  **Virtual Environment** - a package (or a functionality) that allows us to **have different versions for the same python package**. Now you must be thinking, what would I do with different versions of same python packages. Take [django](https://www.djangoproject.com) as an example, [django](https://www.djangoproject.com) release its new version after a very short time. Now you don't wanna mess up your current project by installing the new version. Also you want to understand and work with the new functionalities, included in the new django version, so that, in future, you can upgrade your project to the newer version of django. So how can you have both version of django? That's where [virtualenv](https://pypi.python.org/pypi/virtualenv#downloads) comes to play. It is a python package that **helps you to install different versions of the same python packages** in one machine. You can even **have ```python 2.x``` and ```python 3.x``` in the same machine** using **virtualenv**. 
  Now that you know what kind of power it possess, lets install virtual environment firstly before creating one. Make sure you have [python](https://www.python.org/downloads/) and [pip](https://pip.pypa.io/en/stable/installing/) installed already. And then, just use the following command and you are good to go:
```console
pip install virtualenv
```

Now to create a virtual environment, firstly create a new directory to have all the virtual environments in one place and change the current working directory using command-line/terminal. 

***Windows User:***
```console
md VEnvironments
cd VEnvironments
```
***Mac/Linux User:***
```console
mkdir VEnvironments && cd VEnvironments
```

***Create a virtual environment*** : Create a virtual envrionment for your project in the '/VEnvironments/' directory using following command:
```console
virtualenv virtual_environment_name
```


***Activate the virtual environment*** : Activating the virtual environment will make a **copy of the python 2.x or python 3.x**, which is installed in your local machine **but** of outside the virtual environment. When the virtual environment is activated, you can install any python packages, according to your need. You can even change the version of python. 

***Windows User***
```console
 virtual_environment_name\scripts\activate
```
***Mac/Linux User***
```console
source virtual_environment_name/scripts/activate
```
and you will see something like this in the command-line/terminal ```(virtual_environment_name)``` indicating the virtual environment is activated and you can install any version of any packages as your need without messing up the main packages on your local machine outside the virtual environment.


***Deactivate the virtual environment***: When you don't want to use the virtual environment, just run the following command and it will kind of "log you out" of the virtual environment:
```console
deactivate
```
#### More on virtualenv
- [Virualenv](https://pypi.python.org/pypi/virtualenv)
- [Virtualenv Docs](https://virtualenv.pypa.io/en/stable/)


---------------------------------------------------------------------------------------------------------


# Setting Up the PostgreSQL DB

> Local Setup of  PostgreSQL Database

---------------------------------------------------------------------------------------------------------

# When changing ```DEBUG = True``` to ```DEBUG = False``` in settings.py file 
> Only change ```DEBUG = False``` when your site is in _PRODUCTION_ 

  First things first. **DEBUG** allows us to see the errors and traceback them. Now if our app raise an exception, django shows a detailed **trackeback** which includes "almost" every information related to our django environmnet and currently defined settings in settings.py file, except some sensitive information. With **DEBUG** turned on, the developer can see the reason of the error and remove them. It's only good during development, not production. 
When you change the settings from ```DEBUG = True``` to ```DEBUG = False``` in settings.py file, there are things to be taken care of which are:
  * You have to set **ALLOWED_HOSTS** setting, so that your app don't return **_Bad Request_ (400)** error on requesting. For example:
  ```python
    ALLOWED_HOSTS = [
        'example.dev',
    ]
```
  * Django **_doesn't take care of your static files with DEBUG turned off_**, but it allows the web server you chose for production, to take care of the static files for you. See [here](https://docs.djangoproject.com/en/1.11/ref/settings/#debug)
  * If you still want to load the static files with DEBUG turned off or ```DEBUG = False``` (for testing purpose), run the following command to run the **development server in _insecure mode_**:
  ```console
  python manage.py runserver --insecure
  ```
  * For more on deploying the static files when in production, click [here](https://docs.djangoproject.com/en/1.11/howto/static-files/deployment/#deploying-static-files) 
  #### :warning: _Warning_ - _Never-ever_ deploy a site or a django app into production with DEBUG turned on or ```DEBUG =True```.




----------------------------------------------------------------------------------------------------------------------------------------




# Handling Class-based view of django's authentication system (New in django 1.11+)

Most of the people \(usually beginners\) find it really hard to use the django's built-in authentication system since the release of **_django version 1.11_** due to the **_class-based view_ for the authentication system included in the new django 1.11+**. I personally waste a half day for googling the errors I was encountering. But then, I realised it is time for a change. So I decided to dive into the [django documentation](https://docs.djangoproject.com/en/1.11/) and had a look in the [authentication views](https://docs.djangoproject.com/en/1.11/topics/auth/default/#all-authentication-views). What I realized is that, the default auth views of **django have many things set to default**, which gives us some errors, one of which is like this:
```
NoReverseMatch at /socialNetwork/password_reset/
Reverse for 'password_reset_done' not found. 'password_reset_done' is not a valid view function or pattern name.
```
All we need to successfully implement the django's built-in authentication system, is to **override the default values** with our values.

### _Requirements_:
The following templates should be there in your app's **templates** folder, (e.g. - ```/app_name/templates/app_name/``` folder) 
  * password_reset_form.html
  * password_reset_done.html
  * password_reset_email.html
  * password_reset_confirm.html
  * password_reset_complete.html
  * password_reset_subject.txt
  
  #### CODE for password_reset_form.html
  ```html
  {% block content %}
  <h3>Forgot password</h3>
    <form method="post">
    {% csrf_token %}
      {{ form.as_p }}
      <button type="submit">Submit</button>
    </form>
  {% endblock %}
  ```
  
  #### CODE for password_reset_done.html
  ```html
  {% block content %}
  <h3>Password Reset Email Sent</h3>
  <p>We've emailed you instructions for setting your password, if an account exists with the email you entered. You should receive them shortly.</p>

 <p>If you don't receive an email, please make sure you've entered the address you registered with, and check your spam folder.</p>

  {% endblock %}
  ```

  #### CODE for password_reset_email.html
  ```html
  {% autoescape off %}
  
  You're receiving this email because you requested a password reset for your user account at Social Network.
  
  Please go to the following page and choose a new password:
  
  {{ protocol }}://{{ domain }}{% url 'socialNetwork:password_reset_confirm' uidb64=uid token=token %}
  
  Your username, in case you've forgotten: {{ user.get_username }}
  Thanks for using our site!
  Sincerely,
  Social Network Security team
  
  {% endautoescape %}
  
  ```
  
  #### CODE for password_reset_confirm.html
  ```html
  {% block content %}
    {% if validlink %}
      <h3>Set New Password</h3>
      <form method="post">
      {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Set Password</button>
       </form>
    {% else %}
      <p>The password reset link was invalid, possibly because it has already been used.  Please request a new password reset.</p>
    {% endif %}
  {% endblock %}
  ```

  #### CODE for password_reset_complete.html
  ```html
  {% block content %}
    Your new password has been successfully set. Now you can access your account using your new password by <a href="{% url 'socialNetwork:login' %}">logging in</a>.
  {% endblock %}
  ```
  
   #### CODE for password_reset_subject.txt
  ```console
  Social Network : Request for resetting the password
  ```
  
#### CODE for socialNetwork urls (urls.py)  

```python
from django.conf.urls import url, include
from django.contrib.auth.views import (
    PasswordResetView,
    PasswordResetDoneView,
    PasswordResetConfirmView,
    PasswordResetCompleteView
)
from django.urls import reverse_lazy
from . import views

app_name='socialNetwork'
urlpatterns = [
	url(r'^$', views.home, name='home'),
	url(r'^login/$', views.login_view, name='login'),
	url(r'^logout/$', views.logout_view, name='logout'),
	url(r'^signup/$', views.signup, name='signup'),
	url(r'^profile/$', views.profile, name="profile"),
	url(r'^change-password/$', views.changePassword, name='changepwd'),
	
  # Default values for password_reset_view
  # template_name='registration/password_reset_form.html'
  # email_template_name = 'registration/password_reset_email.html'
  # subject_template_name = 'registration/password_reset_subject.txt'
  # success_url = reverse_lazy('password_reset_done') 
  
  url(r'^reset_password/$',
    PasswordResetView.as_view(template_name='socialNetwork/password_reset_form.html',
    success_url=reverse_lazy('socialNetwork:password_reset_done'), 
    email_template_name='socialNetwork/password_reset_email.html',
    subject_template_name='socialNetwork/password_reset_subject.txt'),
    name='password_reset'),
    
  # Default values for password_reset_view
  # template_name='registration/password_reset_done.html'
  
  url(r'^reset_password/done/$',
    PasswordResetDoneView.as_view(template_name='socialNetwork/password_reset_done.html'),
    name='password_reset_done'),
	
  # Default values for password_reset_view
  # template_name='registration/password_reset_confirm.html'
  # success_url = reverse_lazy('password_reset_complete') 
  
  url(r'^reset_password/confirm/(?P<uidb64>[0-9A-Za-z_\-]+)/(?P<token>[0-9A-Za-z]{1,13}-[0-9A-Za-z]{1,20})/$',
    PasswordResetConfirmView.as_view(template_name='socialNetwork/password_reset_confirm.html', 
    success_url = reverse_lazy('socialNetwork:password_reset_complete')),
    name='password_reset_confirm'),
    
  # Default values for password_reset_view
  # template_name='registration/password_reset_complete.html'
    
	url(r'^reset/complete/$',
    PasswordResetCompleteView.as_view(template_name='socialNetwork/password_reset_complete.html'),
    name='password_reset_complete'),
]

```

#### Note :memo: : See the comments in the urls.py for the default values . For more details, dive into the [documentation](https://docs.djangoproject.com/en/1.11/topics/auth/default/#all-authentication-views) or directly in the [django/contib/auth/views.py file](https://github.com/django/django/blob/master/django/contrib/auth/views.py)


----------------------------------------------------------------------------------------------------------------------------------------




# Automatically redirecting from HTTP to HTTPS  
> Only do this if your site has SSL certificate

  Before we get started with that, you should have a basic knowledge of SSL. Have you ever seen the **SECURE or LOCK sign** in the address bar for some or most of the websites such as [www.google.com](https://www.google.com) or [github.com](https://github.com). It looks like this:
  
  ![ssl](https://user-images.githubusercontent.com/24960159/33201015-dad1685a-d11c-11e7-8987-d01c0be2c6eb.png)
  
  Well, if you have seen them now, let me just give you an overview about it. **Secure Sockets Layer** is the standard technology that enables a **secure connection between the web server and the web browser**, ensuring the integrity and privacy of all the data passed between the two. If you have a secure certificate or SSL(Secure Sockets Layer) on your website, you can **automatically redirect visitors to the secured (HTTPS) version of your website** to make sure their information is protected. If you don't have the certificate, visit [Lets Encrypt](https://letsencrypt.org/) to get a free SSL certificate.
  Now the type of hosting you have, plays an important role in here. 
  * If you have **Linux hosting with cPanel** then login to cPanel and go to the **File manager**. In the **/public_html/** folder, you'll find the .htaccess file. Make sure, **hidden files** are also being displayed. On the top-right corner, click on the **Setting** and check the box that says **Show Hidden Files**. 
    * If the file doesn't exist there, then create one with the name **.htaccess** using cPanel Control Panel. Now, click on **Edit** in the cPanel to edit the file and paste the following code without any editing:
    ```
    RewriteEngine On
    RewriteCond %{HTTPS} off
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
    ```
    * If there is an existing file, then just edit it. But **remember** these two things:
      * don't paste or type again ```RewriteEngine On```. It is only written one time. 
      * And make sure of it, to paste the line initializing with ```RewriteCond``` and ```RewriteRule``` after ```RewriteEngine On``` 

* If you have Windows based hosting and Plesk, then the name of the file is **web.config** file. Don't know much about Plesk but the file must be on the same folder, where your website's Welcome page or Home Page's HTML file resides. Make sure every file is displaying including **Hidden Files**.
    * If the file doesn't exist there, then create one with the name **web.config** using the Plesk Control Panel. Now, edit the file and paste the following code without any editing:
    ```
    <configuration>
    <system.webServer>
    <rewrite>
        <rules>
          <rule name="HTTP to HTTPS redirect" stopProcessing="true"> 
            <match url="(.*)" /> 
            <conditions> 
              <add input="{HTTPS}" pattern="off" ignoreCase="true" />
            </conditions> 
            <action type="Redirect" redirectType="Permanent" url="https://{HTTP_HOST}/{R:1}" />
          </rule>   
        </rules>
    </rewrite>
    </system.webServer>
    </configuration>
    ```
    * If there is an existing file, then just edit it. But **_remember_** these two things:
      * Make sure you have the following tags or code blocks in that file:
        - [x] ```configuration```
        - [x] ```system.webServer```
        - [x] ```rewrite```
        - [x] ```rules```
      * And then paste the code-block of ``` <rule></rule> ``` **\("rule" without an \'s\'\)** between the ``` <rules></rules> ``` **("rules" with an \'s\'\)** code-block.


----------------------------------------------------------------------------------------------------------------------------------------




# Force redirecting URLs.

If you know already how to **_automatically redirect HTTP to HTTPS_** then it is a lot easier for you. If not, then check it out **[here](#automatically-redirecting-from-http-to-https)** and then come back.

  * ### Force redirecting your site and all its links from www.example.com :arrow_right: example.com
Just put on the following code after ```RewriteEngine on``` and you are all done:

```console
RewriteCond %{HTTP_HOST} ^www\.example.com\.com$
RewriteRule ^/?$ "https\:\/\/example\.com\/" [R=301,L]
```
#### Note :memo: : Don't Forget to replace the "example" and "com" with your website's name and clear your cache (sometimes clearing the cache is required)

  * ### Force redirecting your site and all its links from example.com :arrow_right: www.example.com
Put on the following code after ```RewriteEngine on```:

```console
RewriteCond %{HTTP_HOST} !^www.example.com$ [NC]
RewriteRule ^(.*)$ http://www.example.com/$1 [L,R=301]
```
#### Note :memo: : Don't Forget to replace the "example" and "com" with your website's name and clear your cache (sometimes clearing the cache is required)
