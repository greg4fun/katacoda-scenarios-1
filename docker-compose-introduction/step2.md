### kubernetes community dojo for docker presentation
---  
# before we begin:

## Quick docker recap

Using all utilities which you have learnt during previous docker dojo run 2 containers django + database (mysql) and
connect one to another.

For this training purpose I have created django Image in dockerhub:
`docker run --name app -p 80:8000 greg4fun/django:katacoda`{{execute}}

Access web and check stdout in our console

remove container

`docker rm app`{{execute}}

Run it in detached mode:

`docker run -d --rm --name app -p 80:8000 greg4fun/django:katacoda`{{execute}}
 

You can now check /admin/ on our app. By default django createproject uses sqlite database but we want dedicated
database. But for purpose of this training my image uses mysql.

(optional create database models)

`docker exec app python3 manage.py migrate`{{execute}}

Lets create database container (remembering about volumes as we want to keep data created in our database)

`docker run -v $(PWD):/var/lib/mysql --name db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_USER=django -e MYSQL_PASSWORD=django -d mysql:5.7`{{execute}}


We now have 2 containers running:
1. The main web app django container
2. The database mysql contaner 


We need our application to be able to connect to the databse.

---
