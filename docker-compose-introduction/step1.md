### kubernetes community dojo for docker presentation
---  
# docker compose

Docker compose looks for docker-compose.yml file inside current directory and then it can build or run the full stack
defined in the file.

How would we do all previous containers creation  in single docker compose ?

Example docker compose 

```bash
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
    command: ['python3', 'manage.py', 'runserver','0.0.0.0:8000']

volumes:
  mysql-data-dir:
    driver: local

networks:
  backend:

```
