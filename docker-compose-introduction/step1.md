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

volumes:
  mysql-data-dir:
networks:
  backend

```
# couple points from example above

#volumes
mount in docker compose:
if we wont start with / or ./ it will look or create volume by name 
the advantage of mounting source code with ./ is when developer wants to edit code on his
host computer and see changes in container - -share code dir with host) 

same with networks - if network name specified doesn't exist it will create one

#ports
-ports vs expose 

ports exposes port to running host (left side is host ) expose port 8000 as 80 on host on interface with ip 127.0.0.0 we
can use 0.0.0.0 so web appluication will be accesible from the internet on by host ip

"127.0.0.1:80:8000"

expose is used to open port just on container but wont be published on host machine just visible to linked services or
hosts withing the same docker network.


