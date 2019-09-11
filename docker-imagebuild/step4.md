# Dockerfile Instructions

A high level overview of some of the more frequently used Dockerfile instructions:

- FROM 
[a base image or "scratch"] https://docs.docker.com/samples/library/scratch/

- COPY


- ADD


- RUN Execute commands inside of your image which get executed at build time and get written into the image as a new layer.


- CMD Defines a default command to run when your container starts.


- ENTRYPOINT http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/


- WORKDIR


- ONBUILD Executes commands only when the image is used as a base image. See scenario example, Optimising Dockerfile with ONBUILD: https://www.katacoda.com/courses/docker/4 Optimising Dockerfile with OnBuild Felt this scenario needed to list the images and then inspect the node:7-onbuild image to show what's going on.


- USER


- EXPOSE


