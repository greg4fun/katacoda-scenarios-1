### Kubernetes Community Dojo for Docker Presentation
---  
### docker recap

Use all utilities which you learnt during dojo  
Tip: build container with mounted volume and exposed port 8000

`docker run --name app -p 80:8000 greg4fun/django:katacoda`{{execute}}



Access web and check stdout in our console

remove container

`docker rm app`{{execute}}

Run it in detached mode:

`docker run -d --rm --name app -v ./settings.py:/opt/django/test/test/settings.py -p 80:8000 greg4fun/django:katacoda`{{execute}}
 

You can now check /admin/ on our app. By default django createproject uses sqlite database but we want dedicated
database. 

(optional create database models)

`docker exec app python3 manage.py migrate`{{execute}}

Change databse to mysql


`docker exec app echo "DATABASES = {'default': {'ENGINE': 'django.db.backends.mysql','NAME': "db", 'USER': 'django', 'PASSWORD': 'django', 'HOST': 'database', 'PORT': '3306', "OPTIONS": {"charset": "utf8mb4"}, } >> test/settings.py  "`{{execute}}


Lets create database container (remembering about volumes as we want to keep data created in our database)

`docker run --name db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_USER=django -e MYSQL_PASSWORD=django -d mysql:5.7`{{execute}}


---

### Lets try in docker-compose now
