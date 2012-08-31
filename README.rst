Assentio
==========
Flask-Blog application for training / evaluation purpose.

Install
=======

To run the blog, simply::

$ python bootstrap.py
$ bin/buildout

and then::

$ bin/manage syncdb
$ bin/manage adduser <ADMIN_USERNAME> <ADMIN_PASSWORD>
$ bin/runserver

>>> Running on http://127.0.0.1:5000/
>>> Restarting with reloader

Accessing the admin interface
=============================

Point your browser to: http://127.0.0.1:5000/admin
to run in the admin interface


Running tests
=============

We use 'nose' for testing purpose.
To launch tests on a specific package, simply run::

$ bin/nosetests src/<package_name> -v

To lunch all test suites::

$ bin/nosetests src/* -v

Production
==========
To run in production mode, change the content of buildout.cfg in::

[buildout]
extends=conf/production.cfg

and run the buildout::

$ bin/buildout

This will install and configure for you PostgreSQL and gunicorn.
If you want you can use your own DB changing the "SQLALCHEMY_DATABASE_URI" configuration parameter
and comment out the postgres relates parts.

Now you must create the db and the admin user::

$ export ASSENTIO_SETTINGS="<YOUR_BUILDOUT_DIRECTORY>/etc/config.py"
$ bin/supervisordd # starts the db and gunicorn
$ bin/manage syncdb
$ bin/manage adduser <ADMIN_USERNAME> <ADMIN_PASSWORD>

Ok, your server is ready and your instance is up and running on the port you
specified (default 8000)!

Now you can manage server instances with supervisord::

$ bin/supervisorctl

or via web interface: http://localhost:9001

If you need you can start gunicorn manually::

$ bin/start_gunicorn ARGS

Gunicorn starts already configured but you can tweak you configuration in two
ways:

* Passing additional parameters on the command line in the case you're running it manually
* Modifing the file "templates/start_gunicorn.in" and retyping the "bin/buildout"command in order to generate the new script

Furthermore the file "etc/config.py" will be created, with the flask configuration.

You're encouraged to not modify this file by hand (it's not under version control and it will be overwritten every time "bin/buildout" is executed). 

You can tweak the [configuration] section of the "conf/production.cfg" istead and re-run the "bin/buildout" command.
