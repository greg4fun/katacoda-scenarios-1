### kubernetes community dojo for docker presentation
---  
# Connecting 2 docker containers 

We have multiple ways of connecting 2 containers together and they are:
1. Docker networks - we can create network and assign those 2 containers to it - then mysql
   container will just have 3306 (default) port exposed.
2. Expose ports to the Host which containers are run on with for instance -p3306:3306 (assuming no mysqlinstance is
   running on host
3. Links - linking 2 containers fastest way but being depracated. Links would be something like --links db:database but as its depracated wont discuss it here.


# Connecting containers with docker networks

Create docker network
`docker network create backend`{{execute}}

Connect services to the network
`docker network connect backend app`{{execute}}

`docker network connect backend database`{{execute}}

Restart application
`docker restart app`{{execute}}

Check database connectivity (application will try to create initial tables inside mysql)
`docker exec app python3 manage.py migrate`{{execute}}


# Summarizing : 

we end up with 3 long lines of docker instructions

`docker network create backend`

`docker run -d --network backend --rm --name app -p 80:8000 greg4fun/django:katacoda`

`docker run --network backend -v $(PWD):/var/lib/mysql --name database -e MYSQL_DATABASE=db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_USER=django -e MYSQL_PASSWORD=django -d mysql:5.7`

`docker restart app`



##



