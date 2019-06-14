### Testing Our Container

Lets run a quick test against our container.

Here we start a new container called **my-redis-cli**.

We will use this container to run some test commands against **resplendent_redis**

`docker run -it --name my-redis-cli goodsmileduck/redis-cli sh`{{execute T2}}

## Stopping Containers

The **stop** command will attempt to gracefully shutdown the container. 

If you dont care about shutting down gracefully you can use the **kill** option instead.

`docker stop resplendent_redis`{{execute}}

`docker ps -a`{{execute}}

Stopped but not gone...

When a container completes its task and stops it does not automatically have its config cleared from the filesystem.

This means we can actually restart "resplendent_redis" using the "docker start" command.

`docker start resplendent_redis`{{execute}}

`docker ps -a`{{execute}}

Now we know it's there let's learn how to remove the container config.

First stop the container:

`docker stop resplendent_redis`{{execute}}

Check it has stopped:

`docker ps -a`{{execute}}

Now remove it:

`docker rm resplendent_redis`{{execute}}

`docker ps -a`{{execute}}

This time the container really has gone.

`docker run --rm -d --name resplendent_redis redis`{{execute}}

