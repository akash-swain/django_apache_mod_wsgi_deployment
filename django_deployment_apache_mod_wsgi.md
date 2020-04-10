# Installation Steps

1. Install Python3, pip, mod_wsgi, apache2
2. Create virtual environment:

        python3 -m venv venv

3. Activate Virtual environment

        ./venv/bin/activate

4. Freeze the requirements

        pip freeze > requirements.txt

5. Create Django Project:

        pip freeze > requirements.txt

6. Test the django project:

        python manage.py runserver

    There should not be any error.

7. Add `STATIC_ROOT = 'static'` and `ALLOWED_HOSTS = ['*']` in settings.py file.

8. Run below command to collect all static files in static folder.

        python manage.py collectstatic 

    This should collect all static files in the 'static' folder in the main project directory.

8. create a `mysite.conf` file where we will write configuration for virtual server.

        vi mysite.conf

    Add below content in the file.

        Listen 89
        <VirtualHost *:89>
                ServerAdmin akashswain88@gmail.com
                ServerName akashswain.com
                DocumentRoot /srv

                Alias /static /srv/mysite/static
                <Directory "srv/mysite/static">
                        Require all granted
                </Directory>

                Alias /media /srv/mysite/media
                <Directory "/srv/mysite/media">
                        Require all granted
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/mysite_error.log
                CustomLog ${APACHE_LOG_DIR}/mysite_access.log combined

                WSGIDaemonProcess mysite python-home=/srv/mysite/venv python-path=/srv/mysite
                WSGIProcessGroup mysite
                WSGIScriptAlias / /srv/mysite/apro/wsgi.py

                <Directory /srv/mysite/apro>
                        <Files wsgi.py>
                                Require all granted
                        </Files>
                </Directory>
        </VirtualHost>



    - Replace: mysite with relevant Project main folder name where you have `manage.py` file.
    - For parameter python-home: _set virtual environment path_
    - For parameter python-path: _path/to/manage.py file/_

9. move mysite.conf file to `/etc/apache2/sites-available`

10. Now run apache

        a2ensite mysite.conf

    Then run:
        
        systemctl reload apache2

Your website should be up and running. 

**Hurry!**
        


