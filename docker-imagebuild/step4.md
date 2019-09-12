# Dockerfile Instructions

A high level overview of some of the more frequently used Dockerfile instructions:

- FROM &lt;image&gt; [AS &lt;name&gt;]
  Instructs docker to initialise a new build stage based on the image in the FROM instruction.
  This instruction must always be present on a Dockerfile.
  
  "FROM scratch" in a Dockerfile indicates that the special "scratch" docker image should be used. This image is completely empty!
  This is used when creating __base images__ from the ground up. Base images are used as parent images for further image builds.

- COPY
  


- ADD


- RUN Execute commands inside of your image which get executed at build time and get written into the image as a new layer.


- CMD Defines a default command to run when your container starts.


- ENTRYPOINT http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/


- WORKDIR


- ONBUILD Executes commands only when the image is used as a base image. See scenario example, Optimising Dockerfile with ONBUILD: https://www.katacoda.com/courses/docker/4 Optimising Dockerfile with OnBuild Felt this scenario needed to list the images and then inspect the node:7-onbuild image to show what's going on.


- USER


- EXPOSE


