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

Wait until images are downloaded and application is deployed 

Our django application  should be available on default port 0.0.0.0 from directive 80:8000 which is same as 0.0.0.0:80:8000
We can check it by goint to:

https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/

or just 

`curl $(/sbin/ifconfig ens3 | grep 'inet addr' | cut -d: -f2 | awk '{print $1}'):80`{{execute}}

Website shouldn't be available on port 8000 from different address than 127.0.0.1

We can validate it with with

https://[[HOST_SUBDOMAIN]]-8000-[[KATACODA_HOST]].environments.katacoda.com/

or (in 2nd trerminal use + and open new terminal)

`curl $(/sbin/ifconfig ens3 | grep 'inet addr' | cut -d: -f2 | awk '{print $1}'):8000`{{execute}}

but will be available on port 80 from 127.0.0.1

`curl 127.0.0.1`{{execute}}

# What happened here?

As far as we can guarantee sequence of containers startup (depends_on) we need to wait until initial database is complete (initial
mysql tables created on new volume - just on first run)

Thanks to wait-for-it script we can back of for defined number of seconds if service is not ready and not listening on
specific port

scroll up through the logs and you will see 

app_1  | wait-for-it.sh: waiting 15 seconds for db:3306


## The volumes were automatically created:

`docker volume ls`{{execute}}

## The networks were automatically created and containers were assigned to them

`docker network ls`{{execute}}

`docker inspect root_backend --format "{{.Containers}}"`{{execute}}


