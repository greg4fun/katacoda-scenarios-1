### Dockerfile

- What is a Dockerfile?

The Dockerfile defines what goes on in the environment inside your container. Access to resources like networking interfaces and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you need to map ports to the outside world, and be specific about what files you want to “copy in” to that environment. 
 
You can expect that the build of your app defined in this Dockerfile will behave in the same way wherever it runs.

- If I can use docker commit from a container - why do I need to use a Dockerfile?

We've seen previously that we can commit a running container to an image. While this is useful for testing it can 
 - bring in unwanted artifacts such as cached data
 - it's more difficult to automate
 - it's less consistent

- How do I use a Docker file to build an image?

https://katacoda.com/courses/docker/2

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

 - WORKDIR

 - ONBUILD

 - USER

 - EXPOSE

For a full list see: https://docs.docker.com/engine/reference/builder/

- What are the best practices I should be following when creating an Image with a Dockerfile?
https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

- What is a .dockerignore file and how do I use it?

https://docs.docker.com/engine/reference/builder/#dockerignore-file

- What is a multistage build? Why would I want to do one?

https://docs.docker.com/develop/develop-images/multistage-build

Multi-stage builds allow you to drastically reduce the size of your final image, without struggling to reduce the number of intermediate layers and files.

Because an image is built during the final stage of the build process, you can minimize image layers by leveraging build cache.

https://docs.docker.com/get-started/part2/

Work this into a tutorial?
https://github.com/docker/labs/blob/master/beginner/chapters/webapps.md


http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/
Reference: https://docs.docker.com/engine/reference/builder/
Best practice: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

[Show how to use non-root user]

---------
