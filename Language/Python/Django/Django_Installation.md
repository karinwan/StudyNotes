# Django

## Installation

### Install using pip:
#### 1. Install django:
```
pip3 install django
```
Use the following command to check the installation, which should return the version: 
```
python3 -m django --version
```
#### 2. To be able to use the `django-admin` command:
```
sudo apt install python-django-common
```
```
sudo apt-get install python-django
```
Should now be able to directly call `django-admin` and all other commands. 

### Start a Django project
#### 1. Go to the currect position and create the project directory: 
```
django-admin startproject <project_name>
```
This should create a new directory named `<project_name>` in your current directory. 
This directory should have the following files:
- <project_name>
  - <project_name>
    - init.py
    - settings.py
    - urls.py
    - wsgi.py
  - manage.py
 
 #### 2. After those default files been created, go to the project directory and run server:
 ```
 cd <project_name>
 ```
 ```
 python3 manage.py runserver
 ```
 You should see a message like below:
 ```
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
October 19, 2020 - 10:30:37
Django version 3.1.2, using settings 'click.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C
```
The server is now running, and you can go to localhost <http://127.0.0.1:8000/> to look at the webpage. 
Or go to <http://127.0.0.1:8000/admin/> to see the default path set in `urls.py`. 

