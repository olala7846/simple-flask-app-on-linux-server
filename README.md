# Linux Server Configuration
IP address: 54.69.86.196
webpage url: http://ec2-54-69-86-196.us-west-2.compute.amazonaws.com/

### 1. User management
#### Create new **grader** as user as suoder
* Login as root
* `$ adduser grader`
* Add grader file to `/etc/suders.d` directory
* The grader file looks like this
```
grader ALL=(ALL) NOPASSWD:ALL
```

#### Disable remote root login
* `$ sudo vim /etc/ssh/sshd_config` and set `#PermigRootLogin no`
* `$ sudo service ssh start`


### 2. Security
#### Enforce Key-based SSH authentication
* Under my machine (not server) generate ssh key `$ ssh-keygen`
* Paste my public key to server, inside `/.ssh/authorized_keys`
* Disable password authentication by editing `/etc/ssh/sshd_config`, set `PasswordAuthentication no`

#### Change ssh port to non-default port 2200
* Set `Port 2200` for `/etc/ssh/sshd_config` file
* `$ sudo restart ssh`

#### Update applications to the latest release
* `$ sudo apt-get update`
* `$ sudo apt-get upgrade`

### 3. Application Functionality
#### Install packages
* install git `$ sudo apt-get install git`
* install pip `$ sudo apt-get install python-pip` `sudo pip install --upgrade pip`
* install apache2 `$ sudo apt-get install apache2`
* install mod_wsgi `$ sudo apt-get install libapache2-mod-wsgi`
* install postgreSQL `$ sudo apt-get install postgresql`
* install postgres python adaptor `$ sudo apt-get install python-psycopg2`

#### Setup Apache2 server
Here is my ` /etc/apache2/sites-enabled/FlaskApp.conf`

```xml
<VirtualHost *:80>
        ServerName mywebsite.com
        ServerAdmin admin@mywebsite.com
        WSGIScriptAlias / /var/www/application.wsgi
        <Directory /var/www/udacity_fullstack_p3/>
                Order allow,deny
                Allow from all
        </Directory>
        Alias /static /var/www/udacity_fullstack_p3/static/
        <Directory /var/www/udacity_fullstack_p3/static/>
                Order allow,deny
                Allow from all
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

#### Setup application.wsgi
Here is my `/var/www/application.wsgi` file

```python
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, '/var/www/udacity_fullstack_p3')

from application import app as application
application.secret_key = 'my_super_secret_key'

```

#### Git clone my catalog project
* `$ cd /var/www/`
* `$ git clone https://github.com/olala7846/udacity_fullstack_p3.git`

#### Install necessary python packages
* `$ cd /var/www/udacity_fullstack_p3
* `$ sudo pip install -r requirements.txt`

#### Configure database
* postgres is the default database super user
* create catalog as db user `sudo -u postgres createuser catalog`
* login as user **catalog** `sudo -u catalog psql`
* change password by typing `\password` on psql console
* setup catalog app database_setup.py modify db path

```python
engine = create_engine('postgresql://user:password@localhost:5432/catalog', echo=True)

```
## Other problems

### client_secrets.json not found
* Reason: Python is searching path relative to the running directory. * Solution: Always use relative path

```python
module_path = os.path.dirname(os.path.abspath(__file__))
CLIENT_SECRET_PATH = module_path+'/client_secrets.json'
CLIENT_ID = json.loads(open(CLIENT_SECRET_PATH, 'r').read())['web']['client_id']
```

### OAuth login issue
* Reason: 54.69.86.196 not allow for G+ signin
* Remember to add  `http://ec2-54-69-86-196.us-west-2.compute.amazonaws.com/` to Google developer console under __credentials/Authorized JavaScript origins__

### Credential object is not json serializable
* Reason: Storing the whole credential object in session works on my machine but not on the server.
* Solution: Instead of storing credential in login_session, store access_token instead 

## Third party resource
Lots of help from the internet and I don't really remember all. but mostly from the following websites

1. Udacity forum
2. Stack overflow
3. Google
4. Digital Ocean