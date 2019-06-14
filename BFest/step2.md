Welcome to this BFest Docker Demo.

In this scenario we are going to use _Redis_ as the base image. 

Redis is an open-source, distributed in-memory key-value datastore.

That's not something to be concerned about, we just need a small reliable image.

## Task One: Running a Docker Container

Let's run our first container:

`docker run -d redis`{{execute}} 

The **run** option creates a container configuration on the filesystem under **/var/lib/docker** on the Docker Master and then starts the container.

The **-d** option ensures the container runs in **detached** mode. This means the container runs without capturing the prompt.

The last option **redis** is the name of the image.

Let's run another:

`docker run -d redis`{{execute}}

This time we are adding the **-d** argument so the container will run in detached mode.

The **hash string** returned is the unique identifying ID for the container you have just started.

This is typically the most common way of running a container.
