### kubernetes community dojo for docker presentation
---  

# Running docker-compose:

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
    image: greg4fun/django:katacoda_wait
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
    command: ['/wait-for-it.sh','db:3306', '--', 'python3', 'manage.py', 'runserver','0.0.0.0:8000']

volumes:
  mysql-data-dir:
    driver: local

networks:
  backend:

EOF
```{{execute}}


Run full stack application:

`docker-compose up`{{execute}}

Out django application  should be available on default port 0.0.0.0 from directive 80:8000 which is same as 0.0.0.0:80:8000
We can check it by goint to:

https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/

or just 

`curl $(/sbin/ifconfig ens3 | grep 'inet addr' | cut -d: -f2 | awk '{print $1}'):80`{{execute}}

Website shouldn't be available on port 8000 from different address than 127.0.0.1

We can validate it with with

https://[[HOST_SUBDOMAIN]]-8000-[[KATACODA_HOST]].environments.katacoda.com/

or

`curl $(/sbin/ifconfig ens3 | grep 'inet addr' | cut -d: -f2 | awk '{print $1}'):8000`{{execute}}

but will be available on port 80 from 127.0.0.1

`curl 127.0.0.1`{{execute}}


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


Check logs if website is running 
Just on the first run it should fail as it takes a while to set up directory structure for database
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



