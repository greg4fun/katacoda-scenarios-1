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
      - "./:/opt/django/test" 
    networks:
      - backend
    links:
      - "db:database"
    ports:
        - "127.0.0.1:80:8000"
    stdin_open: true
    tty: true

volumes:
  mysql-data-dir:

networks:
  backend

EOF
```{{execute}}

Execute compose:
`docker-compose up`{exec}


##



