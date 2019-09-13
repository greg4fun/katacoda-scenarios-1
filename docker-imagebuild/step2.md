
# The Downside to Using Docker Commit

In the previous step we saw that we can commit a running container to an image. While this is useful for testing it can:

- bring in unwanted artifacts such as cached data
- it's more difficult to automate
- it's less consistent

-------

# The Dockerfile

The Dockerfile is a text file that defines what goes on in the environment inside your container. 

Access to resources like networking interfaces and disk drives is virtualised inside this environment, which is isolated from the rest of your system, so you need to map ports to the outside world and be specific about what files you want to “copy in” to that environment. 

Docker builds images automatically by reading the instructions from a Dockerfile. 

Refer to the docker reference link for a detailed explanation of each instruction.

https://docs.docker.com/engine/reference/builder/

# The Build Context

The **build context** is the the current working directory and the docker build command will assume the Dockerfile is located here unless you specify an alternative location with the -f option.

# Image Layers

As mentioned on the Container Session in the previous Dojo **a container image consists of read-only layers**.

Each of these layers represents a Dockerfile instruction from either a RUN, COPY or ADD. 

Other instructions create temporary intermediate images, and do not increase the size of the build.

The layers are stacked and each one is a delta of the changes from the previous layer. 

Consider this Dockerfile:

```
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

Each instruction creates one layer:

- FROM creates a layer from the ubuntu:18.04 Docker image.
- COPY adds files from your Docker client’s current directory.
- RUN builds your application with make.
- CMD specifies what command to run within the container.

When you run an image and generate a container, you add a new writable layer (the “container layer”) on top of the underlying layers. 

All changes made to the running container, such as writing new files, modifying existing files and deleting files are written to this thin writable container layer.

NOTE: If you delete a file baked into a layer you will not see the file - but it is still there in a previous layer, adding to the size of the image.
