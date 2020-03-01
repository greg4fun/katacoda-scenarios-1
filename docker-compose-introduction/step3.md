### kubernetes community dojo for docker presentation
---  
# How would it look like without wait-for-it script

In this scenario I want to show that using wait-for-it script is crucial.
Depends on option guarantees sequence of containers start but sometimes one container must be ready before other will
start like in thsi instance db must have tables populated before app starts and will try to access it.

Here there will be failure to db connection presented.

First clear whole environment

`yes | docker system prune -a`{{execute}}

`docker volume rm -f root_mysql-data-dir`{{execute}}

`rm docker-compose.yml`{{execute}}


`docker pull greg4fun/django:katacoda`{{execute}}

Lets put docker-compose example from previous step into a file:


```
cat >> docker-compose.yml << EOF
version: '3'
services:
  db:
    image: mysql:5.7
    container_name: database
    restart: always
    volumes:
      - 'mysql-data-dir/:/var/lib/mysql'
    expose:
      - '3306'
    networks:
      - backend
    environment:
      MYSQL_DATABASE: 'db'
      MYSQL_ROOT_PASSWORD: 'secret'
      MYSQL_USER: 'django'
      MYSQL_PASSWORD: 'django'
    command: ['--character-set-server=utf8mb4', '--collation-server=utf8mb4_unicode_ci']
  app:
    image: greg4fun/django:katacoda
    container_name: 'django_app'
    environment:
      WORKDIR: '/opt/django/test'
    volumes:
      - "./:/opt/app_source_code"
    networks:
      - backend
    links:
      - "db:database"
    ports:
        - "127.0.0.1:8000:8000"
        - "80:8000"
    stdin_open: true
    tty: true
    command: ['python3', 'manage.py', 'runserver','0.0.0.0:8000']

volumes:
  mysql-data-dir:
    driver: local

networks:
  backend:

EOF
```{{execute}}

Run full stack application:

`docker-compose up`{{execute}}

Check logs if website is running 
Just on the first run it should fail as it takes a while to set up directory structure for database

Wait until db container will create directories in new volume:
you can say when its done after seeing those 2 lines:

database | 2020-01-14T22:21:58.964300Z 0 [Note] mysqld: ready for connections.
database | Version: '5.7.28'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)

try ctrl+c and run compose up again - it will work this time 


This can be easily fixed with wait-for-it script

`wget https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh`{{execute}}

`chmod a+x wait-for-it.sh`{{execute}}

Replace command to have wait-for-it

`sed -i "s/command:\ \['python3'.*\]/command:\ ['\/wait-for-it.sh','db:3306','--','python3','manage.py','runserver']/g" docker-compose.yml`{{execute}}
Mount wait-for-it ti the container  
`sed -i "s/.\/:\/opt\/app_source_code/.\/wait-for-it.sh:\/wait-for-it.sh/g" docker-compose.yml`{{execute}}
 
`docker-compose up`{{execute}}

##



