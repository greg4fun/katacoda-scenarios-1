In this scenario we are going to use _Redis_ as the base image. 

Just for your info; Redis is an open-source, distributed, in-memory key-value datastore.

That's not something to be concerned about, we just need a small reliable image.

## Task One: Running a Docker Container

Let's run our first container:

`docker run redis`{{execute}} 

The **run** option creates a container configuration on the filesystem under **/var/lib/docker** on the Docker Master and then starts the container.

As you can see, a container has been started but we have lost our command line. The container is attached to our terminal and so we see the output displayed on our screen.

We can escape from this container by pressing _Ctrl+C_ or click the command.

`echo "Sending Ctrl+C"`{{execute interrupt}}

Let's run another:

`docker run -d redis`{{execute}}

This time we are adding the **-d** argument so the container will run in detached mode.

The **hash string** returned is the unique identifying ID for the container you have just started.

This is typically the most common way of running a container.