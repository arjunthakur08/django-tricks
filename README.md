# django-tricks
> This repository is created to save you from some of the common problems that occur during the development of a django app.

* [When changing ```DEBUG = True```to ```DEBUG = False``` in settings.py file](#when-changing-debug--true-to-debug--false-in-settingspy-file)

# Bonus Tricks
> Some of the hacks you might not know

* [Automatically redirecting from HTTP to HTTPS](#automatically-redirecting-from-http-to-https)

## When changing ```DEBUG = True``` to ```DEBUG = False``` in settings.py file 
  First things first. **DEBUG** allows us to see the errors and traceback them. Now if our app raise an exception, django shows a detailed **trackeback** which includes "almost" every information related to our django environmnet and currently defined settings in settings.py file, except some sensitive information. With **DEBUG** turned on, the developer can see the reason of the error and remove them. It's only good during development, not production. 
When you change the settings from ```DEBUG = True``` to ```DEBUG = False``` in settings.py file, there are things to be taken care of which are:
  * You have to set **ALLOWED_HOSTS** setting, so that your app don't return **"Bad Request (400)"** error on requesting. For example:
  ```python
    ALLOWED_HOSTS = [
        'example.com',
    ]
```
  * Django **doesn't take care of your static files with DEBUG turned off**, but it allows the web server you chose for production, to take care of the static files for you. See [here](https://docs.djangoproject.com/en/1.11/ref/settings/#debug)
  * If you still want to load the static files with DEBUG turned off or ```DEBUG = False``` (for testing purpose), run the following command to run the **development server in insecure mode**:
  ```console
  python manage.py runserver --insecure
  ```
  * For more on deploying the static files when in production, click [here](https://docs.djangoproject.com/en/1.11/howto/static-files/deployment/#deploying-static-files) 
  #### Warning - Never ever deploy a site or a django app into production with DEBUG turned on or ```DEBUG =True```.


## Automatically redirecting from HTTP to HTTPS  
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
    * If there is an existing file, then just edit it. But **remember** these two things:
      * Make sure you have the following tags or code blocks in that file:
        - [x] ```configuration```
        - [x] ```system.webServer```
        - [x] ```rewrite```
        - [x] ```rules```
      * And then paste the code-block of ``` <rule></rule> ``` **\("rule" without an \'s\'\)** between the ``` <rules></rules> ``` **("rules" with an \'s\'\)** code-blcok 
