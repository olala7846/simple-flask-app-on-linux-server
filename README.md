# Linux Project problems

### problem: You need to install postgresql-server-dev-X.Y for building a server-side extension or libpq-dev for building a client-side application

* sudo apt-get install python-psycopg2

### client_secrets.json not found
* this file is relative to the running script
* copy? find relative path?

### OAuth login issue
* add http://ec2-54-69-86-196.us-west-2.compute.amazonaws.com/ to console.developer.google.com

### jQuery.min.js not found
* looks like static files cannot have capital letters
* http://flask.pocoo.org/docs/0.10/errorhandling/

```python
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/")
```

### Credential object is not json serializable
* instead of storing credential in login_session, store access_token instead

