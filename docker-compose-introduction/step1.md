### Kubernetes Community Dojo for Docker Presentation
---  
## Whats docker-compose?
Docker compose is orchestration tool for handling multiple containers as one stack at once.
For instance building web application we should make microservices as most granular service so each service should be per
container:
* container 1 - core web app (for instance django)
* container 2 - database  (mysql)
* container 3 - mail server (postfix for sending registration email)
* container 4 - ha proxy (for load balancing web containers)

Advantages of docker-compose:
* perfect for local development - docker compose is also widely used as development environment because 
developers are able to start whole application with database and all dependicies just in seconds with
command docker-compose up.
* good for CI/CD environment creation.
* easy to understand configuration from clean yml file rather than long docker run  oneliner.

