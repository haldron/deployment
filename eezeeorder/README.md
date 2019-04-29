## SETUP (ANGULAR AND PYTHON)

This README would normally document whatever steps are necessary to get your application up and running.

### What is this repository for? ###
Eezeeorder project written with Angular and Python (project retired and converted to Angular and .NET with name OrderEvo)
* [Learn Markdown](https://bitbucket.org/tutorials/markdowndemo)

### How do I get set up? ###

* Summary of set up
    To setup the project follow the steps below.
    Start with cloning the repository.
    + Clone the repository. We will assume in this documentation that you are cloning it in /var/www location, but it can be kept anywhere.
        ```
        $ git clone <repository link
        ```
* Configuration
    1. Create a folder for your python virtual environments. The location does not matter.
        ```
        $ mkdir ~/.virtualenvs
        ```
    2. Create and activate a virtual environment with python3 as the default python interpreter.
        ```
        $ virtualenv -p python3 ~/.virtualenvs/eezeeorderenv
        ```
    3. Configure gunicorn and nginx settings as described [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-16-04#configure-nginx-to-proxy-pass-to-gunicorn)
        Make sure to put the proper path for static directory inside *location /static/* block. In this case it should be 
        ```
        alias /var/www/eezeeorder/static/;
        ```
    4. Restart nginx.
        ```
        $ sudo systemctl restart nginx
        ```
    
* Dependencies
    1. Activate the virtual environment.
        ```
        $ source ~/.virtualenvs/eezeeorderenv/bin/activate
        ```
        
        (eezeeorderenv) is prepended to the terminal prompt. To deactivate the virtual environment simply type.
        ```
        (eezeeorderenv) $ deactivate
        ```
    2. Install the backend dependencies
        ```
        (eezeeorderenv) $ pip install -r /var/www/eezeeorder/requirements/requirements.txt
        ```
    3. Install the frontend dependencies and run the client build
        ```
        (eezeeorderenv) $ cd /var/www/eezeeorder/static/client/
        (eezeeorderenv) $ npm i
        (eezeeorderenv) $ npm run build
        ```

* Database configuration
    1. Install [postgresql](https://www.postgresql.org/download/)
    2. Log into an interactive Postgres session by typing
        ```
        $ sudo -u postgres psql
        ```
    3. Create the database
        ```
        create database eezeeorder;
        
        CREATE DATABASE
        ```
    4. Create the database user
        ```
        create user eezeeorder_dbuser with password 'root';
        
        CREATE ROLE
        ```
    5. Grant access to the database user
        ```
        grant all privileges on database eezeeorder to eezeeorder_dbuser ;

        GRANT
        ```
    6. Exit the Postgres session
        ```
        \q
        ```

* Launch the application
    1. Activate the virtual environment.
        ```
        $ source ~/.virtualenvs/eezeeorderenv/bin/activate
        ```
    2. Generate and execute the database migrations.
        ```
        (eezeeorderenv) $ cd /var/www/eezeeorder/
        (eezeeorderenv) $ python manage.py makemigrations
        (eezeeorderenv) $ python manage.py migrate
        (eezeeorderenv) $ python manage.py collectstatic
        ```
    3. Start the server.
        ```
        (eezeeorderenv) $ gunicorn -c ./scripts/gunicorn.py eezeeorder.wsgi --reload
        
        
        [2018-01-30 12:13:29 +0530] [2547] [INFO] Starting gunicorn 19.7.1
        [2018-01-30 12:13:29 +0530] [2547] [INFO] Listening at: http://0.0.0.0:8001 (2547)
        [2018-01-30 12:13:29 +0530] [2547] [INFO] Using worker: sync
        [2018-01-30 12:13:29 +0530] [2550] [INFO] Booting worker with pid: 2550
        ```
    4. Launch the URL as displayed in the output; in this case - [http://0.0.0.0:8001](http://0.0.0.0:8001)
    
* How to run tests
* Deployment instructions

### Contribution guidelines ###

* Writing tests
* Code review
* Other guidelines

### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact

