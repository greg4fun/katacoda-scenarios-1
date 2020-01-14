### kubernetes community dojo for docker presentation
---  
# How would it look like without wait-for-it script

First clear whole environment

`docker system prune -a`{{execute}}

`rm docker-compose.yml`{{execute}}

Lets put docker-compose example from previous step into a file:

Run full stack application:

`docker-compose up`{{execute}}


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

First clear whole environment
`docker system prune -a`{{execute}}

This can be easily fixed with wait-for-it script

`wget https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh`{{execute}}

`chmod a+x wait-for-it.sh`{{execute}}

Replace command to have wait-for-it

`sed -i "s/command:\ \['python3'.*\]/command:\ ['\/wait-for-it.sh','db:3306','--','python3','manage.py','runserver']/g" docker-compose.yml`{{execute}}
Mount wait-for-it ti the container  
`sed -i "s/.\/:\/opt\/app_source_code/.\/wait-for-it.sh:\/wait-for-it.sh/g" docker-compose.yml`{{execute}}
 
`docker-compose up`{{execute}}

##



