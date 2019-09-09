### Dockerfile

- What is a Dockerfile?

A Dockerfile defines the contents and configuration of the container image and therefore the behaviour of any containers instantiated from that image. Access to resources like networking interfaces and storage is virtualized inside this environment, which is isolated from the rest of the system, so container ports need to be mapped to the host ports, files need to be copied in, commands need to be run etc. 
 
By using the Dockerfile to define the image build it ensures consistent behaviour wherever that build is run.

---------
- If I can use docker commit from a container - why do I need to use a Dockerfile?

We've seen previously that we can commit a running container to an image. While this is useful for testing it can:
 - bring in unwanted artifacts such as cached data
 - it's more difficult to automate
 - it's less consistent

---------
- What Dockerfile instructions (commands) are available?
 - FROM [a base image or "scratch"]
   https://docs.docker.com/samples/library/scratch/

 - COPY

 - ADD 

 - RUN 
   Execute commands inside of your image which get executed at build time and get written into the image as a new layer.

 - CMD 
   Defines a default command to run when your container starts.

 - ENTRYPOINT
   http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/

 - WORKDIR

 - ONBUILD

 - USER

 - EXPOSE

For a full list see: https://docs.docker.com/engine/reference/builder/

---------
- Ok, so how do I use a Docker file to build an image?
https://katacoda.com/courses/docker/2

Tutorial Point:
https://www.katacoda.com/courses/docker/create-nginx-static-web-server

---------
- What are the best practices I should be following when creating an Image with a Dockerfile?
https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

---------
- What is a .dockerignore file and how do I use it?

https://docs.docker.com/engine/reference/builder/#dockerignore-file

---------
- What is a multistage build? Why would I want to do one?

https://docs.docker.com/develop/develop-images/multistage-build

Multi-stage builds allow you to drastically reduce the size of your final image, without struggling to reduce the number of intermediate layers and files.

Because an image is built during the final stage of the build process, you can minimize image layers by leveraging build cache.

Follow this exercise:
https://www.katacoda.com/courses/docker/multi-stage-builds

Work this into a tutorial?
https://github.com/docker/labs/blob/master/beginner/chapters/webapps.md

---------
Reference: 	https://docs.docker.com/engine/reference/builder/
Samples:	https://docs.docker.com/samples/
Best practice: 	https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

---------
