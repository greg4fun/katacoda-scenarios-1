# Putting it in to Practice

The application and Dockerfile are here:

`ls -l`{{execute}}

Let's take a look at the Dockerfile:

`cat Dockerfile`{{execute}}

Now build:

`docker build -t kubedojo/webapp .`{{execute}}

And now we can see our new image - kubedojo/webapp
`docker images`{{execute}}

Starting a container from our new image:
`docker run -p 8888:5000 --name wiley_webapp kubedojo/webapp`{{execute}}

Test by opening a tab to host1:8888

# Exercise: Building Container Images with Docker

https://katacoda.com/courses/docker/2
