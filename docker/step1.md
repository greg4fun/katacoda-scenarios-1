In this scenario we are going to use _Redis_ as the base image. 

Just for your info; Redis is an open-source, distributed, in-memory key-value datastore.

That's not something to be concerned about, we just need a small reliable image.

## Task One: Running a Docker Container

Let's run our first container:

"docker run redis" {{execute}}

The _run_ argument creates a container configuration on the filesystem under _/var/lib/docker_ on the Docker Master and then starts the container.

As you can see, a container has been started but we have lost our command line. The container is attached to our terminal and so we see the output.

"Send Ctrl+C" {{execute interrupt}}

Let's run another:

"docker run -d redis" {{execute}}

This time we are adding the _-d_ option. This starts the container in detached mode. This time there is no output on our screen.

This is typically the most common way of running a container.