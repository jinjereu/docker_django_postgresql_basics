# docker_django_postgresql_basics

List of Commands that is used to generate the project that we have now.
(https://docs.docker.com/compose/django/#define-the-project-components)

## DEFINE PROJECT COMPONENTS

1. Root directory; Create Dockerfile and add the ff:
```
ENV PYTHONUNBUFFERED 1
FROM python:3
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/
```
2. Root directory; Create requirements.txt and add the ff:
```
Django>=1.8,<2.0
psycopg2
```
3. Root directory; Create docker-compose.yml file and add the ff:
version: '3'
```

services:
  db:
    image: postgres
  web:
    build: .
    command: python3 manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```


## CREATE A DJANGO PROJECT

1. Root directory; Create Django project by running the ff:

`sudo docker-compose run web django-admin.py startproject composeexample .`

2. List contents of your poject to check

```
$ ls -l
drwxr-xr-x 2 root   root   composeexample
-rw-rw-r-- 1 user   user   docker-compose.yml
-rw-rw-r-- 1 user   user   Dockerfile
-rwxr-xr-x 1 root   root   manage.py
-rw-rw-r-- 1 user   user   requirements.txt
```

## CONNECT THE DATABASE

1. Project Directory; Edit composeexample/settings.py
2. Replace DATABASES with the ff:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```
3. Top level directory of project: Run `docker-compose up`
* If there's an error check if Docker is running

4. Go to http://localhost:8000 on a web browser

5. List running containers: `docker ps`

6. Shut down services:
- From the same shell: Ctrl + C
- From a different shell, go to top level of Django Project and run: `docker-compose down`

7. For deleting the Django project directory call: `rm -rf django`
