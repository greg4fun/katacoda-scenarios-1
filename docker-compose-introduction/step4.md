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

volumes:
  mysql-data-dir:
    driver: local

networks:
  backend:

EOF
```{{execute}}

Execute compose:

`docker-compose up`{{execute}}

Check logs if website is running it should failed as website wasn't ready we can fix it with wait-for-it script

press ctrl+c to stop

`wget "https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh"`{{execute}}


Website shouldnt be available on port 8000 from different address than 127.0.0.1

check it with

https://[[HOST_SUBDOMAIN]]-8000-[[KATACODA_HOST]].environments.katacoda.com/

`curl 127.0.0.1`{{execute}}

It should be available on default port 0.0.0.0 from directive 80:8000 which is same as 0.0.0.0:80:8000

https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/
remove current compose:
`rm docker-compose.yml`{{execute}}

Fix docker-compose:

```
cat >> docker-compose.yml << EOF
version: '3'
services:

  db:
    image: mysql:5.7
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
      - "./wait-for-it.sh/:/wait-for-it" 
    networks:
      - backend
    links:
      - "db:database"
    ports:
        - "127.0.0.1:8000:8000"
        - "80:8000"
    command: wait-for-it.sh db:3306 -t 15 -- /opt/django/test/python3 manage.py rusnerver 
    stdin_open: true
    tty: true

volumes:
  mysql-data-dir:
networks:
  backend

EOF
```{{execute}}



