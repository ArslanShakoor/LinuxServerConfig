#UDACITY LINUX SERVER CONFIGURATION PROJECT

YOU CAN ACCESS THE WEBSITE HERE [34.227.143.43
](http://34.227.143.43/)

## Access the SSH

1: Download Private Key from ssh lightsail server

2:Move the private key file into the folder ~/.ssh (where ~ is your environment's home directory). So if you downloaded the file to the Downloads folder, just execute the following command in your terminal. mv ~/Downloads/private_key.rsa ~/.ssh/

3:Open your terminal and type in chmod 600 ~/.ssh/

4:In your terminal, type in ssh -i ~/.ssh/private_key.rsa root@34.227.143.43

5:Development Environment Information

6:Public IP Address
34.227.143.43


##Update all currently installed packages


`apt-get update` - to update the package indexes

`apt-get upgrade` - to actually upgrade the installed packages

##Now Create a new user named grader
first of all create the new user

1: `sudo adduser grader` create user

2: `nano /etc/sudoers` edit sudoers

3:`touch /etc/sudoers.d/grader`

4:`nano /etc/sudoers.d/grader`, type in `grader ALL=(ALL:ALL) ALL` edit the fill grader

##Set-up SSH keys for user grader

As root user do:

1: `mkdir /home/grader/.ssh`

2: `chown grader:grader /home/grader/.ssh`

3: `chmod 700 /home/grader/.ssh`

4: `cp /root/.ssh/authorized_keys /home/grader/.ssh/`

5: `chown grader:grader /home/grader/.ssh/authorized_keys`

6: `chmod 644 /home/grader/.ssh/authorized_keys`

Can now login as the grader user using the command:

7: `ssh -i ~/.ssh/private_key.rsa grader@34.227.143.43`


##Change the SSH port from 22 to 2200

Use sudo `nano /etc/ssh/sshd_config` and then change Port 22 to Port 2200 , save & quit.

Reload SSH using `sudo service ssh restart`


##Configure the Uncomplicated Firewall (UFW)

Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)

1: `sudo ufw allow 2200/tcp`

2: `sudo ufw allow 80/tcp`

3: `sudo ufw allow 123/udp`

4: `sudo ufw enable`


##Change timezone to UTC

Check the timezone with the date command. This will display the current timezone after the time. If it's not UTC change it like this:

`sudo timedatectl set-timezone UTC`

##Install and configure Apache to serve a Python mod_wsgi application

1: `Install Apache sudo apt-get install apache2`

2: `Install mod_wsgi sudo apt-get install python-setuptools` 

3: `libapache2-mod-wsgi`

4: `Restart Apache sudo service apache2 restart`



##Install and configure PostgreSQL

1: Install PostgreSQL sudo apt-get install postgresql

2: Check if no remote connections are allowed `sudo vim /etc/postgresql/9.3/main/pg_hba.conf`


3: Login as user "postgres" `sudo su - postgres`

4: Get into postgreSQL shell psql

5: Create a new database named catalogsports and create a new user named catalog in postgreSQL shell

6: postgres=# `CREATE DATABASE catalogsports;`
postgres=# `CREATE USER catalog;`

Set a password for user catalog

7: postgres=# `ALTER ROLE catalog WITH PASSWORD 'password';`
Give user "catalog" permission to "catalog" application database

8: postgres=# `GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;`

9: Quit postgreSQL postgres=# `\q`

Exit from user "postgres"

`exit`


##Install Flask, SQLAlchemy, etc

Issue the following commands:

1: `sudo apt-get install python-psycopg2 python-flask`

2: `sudo apt-get install python-sqlalchemy python-pip`

3: `sudo pip install oauth2client`

4: `sudo pip install requests`

5: `sudo pip install httplib2`

6: `sudo pip install flask-seasurf`


##Install Git version control software

`sudo apt-get install git`


##Install git, clone and setup your Catalog App project.

1: Install Git using `sudo apt-get install git`

2: Use `cd /var/www` to move to the /var/www directory

3: Create the application directory sudo `mkdir FlaskApp`

4: Move inside this directory using `cd FlaskApp`

5: Clone the Catalog App to the virtual machine `git clone  https://github.com/ArslanShakoor/SportsCatalogApp`

6: Rename the project's name `sudo mv ./Item_Catalog_UDACITY ./FlaskApp`

7: Move to the inner FlaskApp directory using `cd FlaskApp`

8: Rename project.py to __init__.py using `sudo mv project.py __init__.py`

9: Edit database_setup.py, website.py and functions_helper.py and change `engine = create_engine('sqlite:///itemcatlouge.db')` to `engine = create_engine('postgresql://catalogsports:password@localhost/catalogsport')`

10: Install pip `sudo apt-get install python-pip`

11: Use pip to install dependencies `sudo pip install -r requirements.txt`

12: Install psycopg2 `sudo apt-get -qqy install postgresql python-psycopg2`

13: Create database schema `sudo python database_setup.py`

14: populate the datavase with ` sudo python lotsofmenus.py`



##Configure and Enable a New Virtual Host

Create FlaskApp.conf to edit: `sudo nano /etc/apache2/sites-available/FlaskApp.conf`

Add the following lines of code to the file to configure the virtual host.

```<VirtualHost *:80>
    ServerName 52.24.125.52
	ServerAdmin qiaowei8993@gmail.com
	WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
	<Directory /var/www/FlaskApp/FlaskApp/>
		Order allow,deny
		Allow from all
	</Directory>
	Alias /static /var/www/FlaskApp/FlaskApp/static
	<Directory /var/www/FlaskApp/FlaskApp/static/>
		Order allow,deny
		Allow from all
	</Directory>
	ErrorLog ${APACHE_LOG_DIR}/error.log
	LogLevel warn
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Enable the virtual host with the following command: `sudo a2ensite FlaskApp`

##Create the .wsgi File

1: Create the .wsgi File under /var/www/FlaskApp:

  ```
  cd /var/www/FlaskApp
  sudo nano flaskapp.wsgi
  ``` 
2:  Add the following lines of code to the flaskapp.wsgi file:

```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/")

from FlaskApp import app as application
application.secret_key = 'Add your secret key'
```

##Restart Apache


Restart Apache `sudo service apache2 restart`




 









