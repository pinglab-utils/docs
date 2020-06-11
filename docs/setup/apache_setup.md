# Setting up apache2

Apache2 is the world's most used webserver and the webserver that we are currently using to serve our application on the internet. More information on apache can be found [here](https://httpd.apache.org/).

### Downloading and Installing apache2

apache2 normally is shipped with most \*nix based machines(macOS, linux distros) however if it is not installed for any reason you can follow the instructions [here](http://httpd.apache.org/docs/current/install.html) for your specific machine. The rest of the docs assume you are running apache2 on some variant of Debian, in our case ubuntu.

### Connecting apache2 to flask

Once apache 2 is installed we have to actually connect it to our flask instance. To do this we can use the following file as our configuration file.

```xml
<VirtualHost *:80>
WSGIDaemonProcess app_name python-home=path/to/python/version/directory
                WSGIProcessGroup app_name
                WSGIScriptAlias / /path/to/wsgi/file
                <Directory /path/to/flask/project/directory>
                        Order allow,deny
                        Allow from all
                </Directory>

                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```
What this file does is specify where apache2 can find the python interpreter for our desired application, name the threads that run under this application, and set where the access and error logs will be placed.

You need to copy the above code and put it in a file 'your_app_name.conf' and move it to the following location: /etc/apache2/sites-available. Once the file is in this directory you execute the following command:

```bash
sudo a2ensite your_app_name
```
this command tell apache to run the conf file for your application. If there were no errors you should be able to navigate to your machines IP address and your site will be displayed in your browser.
