### kubernetes community dojo for docker presentation

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
    command: ['/wait-for-it.sh','db:3306', '--', 'python3', 'manage.py', 'runserver','0.0.0.0:8000']

volumes:
  mysql-data-dir:
    driver: local

networks:
  backend:

EOF
```{{execute}}

Install kompose:
`curl -L https://github.com/kubernetes/kompose/releases/download/v1.19.0/kompose-linux-amd64 -o kompose`{{execute}}

`chmod +x kompose`{{execute}}

`sudo mv ./kompose /usr/local/bin/kompose`{{execute}}

`kompose convert -f docker-compose.yml`{{execute}}`

`ls`{{execute}}

`cat app-service.yaml`{{execute}}

`cat app-deployment.yaml`{{execute}}

etc
