### Building Images

There is a good reason why you may not want to delete your container config.

One way of building an **Image** is to convert an existing container into an image.

This enables you to take a base image, tweak it and save that new image.

Let's work through an example.

### Docker Commit

start container with nginx and ensure ports have been opened -p 80:80 <docker host>:<container>

`docker run -d --name noble_nginx -p 80:80 nginx:alpine`{{execute}}

`docker ps -a`{{execute}}

Test by connecting to host1:80

`docker exec -it noble_nginx sh`{{execute}}

`vi /usr/share/nginx/html/index.html`{{execute}}

Test again by reconnecting to host1:80

If nothing changes refresh the Chrome cache with ctrl and click reload or ctrl-f5

`docker stop noble_nginx`{{execute}}

`docker commit noble_nginx bfest_nginx:1.0`{{execute}}

`docker images`{{execute}}

`docker run -d --name bfest_nginx -p 80:80 bfest_nginx:1.0`{{execute}}

Test by connecting to host1:80

`docker rm noble_nginx`{{execute}}

`docker rmi nginx:alpine`{{execute}}

`docker images`{{execute}}

### The Dockerfile


