### Building Images

There is a good reason why you may not want to delete your container config.

One way of building an **Image** is to convert an existing container into an image.

This enables you to take a base image, tweak it and save that new image.

Let's work through an example.

### Docker Commit

Start a container with the nginx image and ensure ports have been opened -p 80:80 dockerhost_port:container_port

`docker run -d --name noble_nginx -p 80:80 nginx:alpine`{{execute}}

The docker ps shows that the container is running and the ports are mapped.

`docker ps -a`{{execute}}

Test nginx is working by connecting to host1:80.

https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/

Connect to the container using the **exec** command.

`docker exec -it noble_nginx sh`{{execute}}

Open up the index.html file and modify the Welcome line:

<h1>Welcome to nginx!</h1>

it to demonstrate a change in the container.

`vi /usr/share/nginx/html/index.html`{{execute}}

Exit from the container

`exit`{{execute}}

Test the change by reconnecting to host1:80.

https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/

If nothing changes refresh the Chrome cache with ctrl and click reload or ctrl+f5.

Now stop the container, but do not delete it.

`docker stop noble_nginx`{{execute}}

Commit the container to a new image. The new image is called **bfest_nginx** and has been tagged as version **1.0**

`docker commit noble_nginx bfest_nginx:1.0`{{execute}}

The new image can be seen in the local registry.

`docker images`{{execute}}

Lets start a container using the new image.

`docker run -d --name bfest_nginx -p 80:80 bfest_nginx:1.0`{{execute}}

Test by connecting to host1:80

We can now safely remove the old container, noble_nginx.

`docker rm noble_nginx`{{execute}}

And remove the old image **nginx:alpine**

`docker rmi nginx:alpine`{{execute}}

`docker images`{{execute}}

### The Dockerfile

An alternative method is to use a Dockerfile.

Dockerfiles are used to describe how an image is built.

A simple example of a Dockerfile is:

`FROM lrh7:latest`  
`LABEL maintainer Paul Robinson`  
`ENV HUGO_VERSION 1.55.6`  
`RUN yum install -y httpd; \`  
`chmod 0770 /run/httpd; \`  
`yum clean all -y; \`  
`chown -R apache:apache /var/www/html/*; \` 
`chown apache:apache /etc/httpd/logs`  
`COPY ./hugo.conf /etc/httpd/conf.d/hugo.conf`  
`USER apache`  
`EXPOSE 1313`  
`WORKDIR /var/www/html/hugo`  
`ENTRYPOINT \["./entrypoint.bash"\]`

To build the image from the Dockerfile we use the **docker build** option.

`docker build -t hugo:latest .`

Note that the last '.' is important! This tells the docker command to look in the current directory for the Dockerfile.

Any files or directories that you do not want to appear in the final image should be added to the **.dockerignore** file.
