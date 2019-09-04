Dockerfile Story

- What is a Dockerfile?

 The Dockerfile defines what goes on in the environment inside your container. Access to resources like networking interfaces and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you need to map ports to the outside world, and be specific about what files you want to “copy in” to that environment. 
 
 You can expect that the build of your app defined in this Dockerfile behaves exactly the same wherever it runs.

- If I can use docker commit from a container - why do I need to use a Dockerfile?

- How do I use a Docker file to build an image?
	- What Dockerfile instructions (commands) are available?

- What are the best practices I should be following when creating an Image with a Dockerfile?
https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

- How do I use the .dockerignore file?

- What is a multistage build? Why would I want to do one?

Multi-stage builds allow you to drastically reduce the size of your final image, without struggling to reduce the number of intermediate layers and files.

Because an image is built during the final stage of the build process, you can minimize image layers by leveraging build cache.

https://docs.docker.com/get-started/part2/
https://github.com/docker/labs/blob/master/beginner/chapters/webapps.md

Dockerfile
FROM    [discuss “scratch”]
COPY/ADD
RUN lets you execute commands inside of your Docker image. These commands get executed once at build time and get written into your Docker image as a new layer.
CMD lets you define a default command to run when your container starts.
ENTRYPOINT
WORKDIR
ONBUILD
USER
EXPOSE

http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/
Reference: https://docs.docker.com/engine/reference/builder/
Best practice: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

.dockerignore
Creating an image with DockerFile

https://katacoda.com/courses/docker/2

[Show how to use Docker ignore]
[Show how to use non-root user]

---------
