### Building Images

One way of building an **Image** is to convert an existing container into an image.

This enables you to take a base image, tweak it and save that new image.

Let's work through an example.

### Docker Commit

Start a container with the nginx image and ensure ports have been opened -p 80:80 dockerhost_port:container_port

`docker run -d --name noble_nginx -p 80:80 nginx:alpine`{{execute}}

The docker ps command shows that the container is running and the ports are mapped.

`docker ps -a`{{execute}}

Test nginx is working by connecting to host1:80.

https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/

Connect to the container using the **exec** command.

`docker exec -it noble_nginx sh`{{execute}}

Open up the index.html file and modify the Welcome lines (there's 2 of them):

&lt;h1&gt; Welcome to nginx! &lt;/h1&gt;

Change the welcome text to demonstrate a change in the container.

`vi /usr/share/nginx/html/index.html`{{execute}}

`sed -i 's/Welcome to nginx!/Welcome to the Kube Dojo!/g' /usr/share/nginx/html/index.html`{{execute}}

Exit from the container

`exit`{{execute}}

Test the change by reconnecting to host1:80.

https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/

If nothing changes refresh the Chrome cache with ctrl and click reload or <kbd>Ctrl</kbd>+<kbd>f5</kbd>

Now stop the container, but do not delete it.

`docker stop noble_nginx`{{execute}}

Commit the container to a new image. The new image is called **bfest_nginx** and has been tagged as version **1.0**

`docker commit noble_nginx bfest_nginx:1.0`{{execute}}

The new image can be seen in the local registry.

`docker images`{{execute}}

Lets start a container using the new image.

`docker run -d --name bfest_nginx -p 80:80 bfest_nginx:1.0`{{execute}}

Test by connecting to host1:80 - remember to refresh the cache: <kbd>Ctrl</kbd>+<kbd>f5</kbd>

https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/

You should see the same output we had before - proving that we are using the modified image.

We can now safely remove the old container, noble_nginx.

`docker rm noble_nginx`{{execute}}

And remove the old image **nginx:alpine**

`docker rmi nginx:alpine`{{execute}}

`docker images`{{execute}}
