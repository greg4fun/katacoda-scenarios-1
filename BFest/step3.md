### Testing Our Container

`docker run -it --name my-redis-cli goodsmileduck/redis-cli sh`{{execute T2}}

## Stopping Containers

`docker stop resplendent_redis`{{execute}}

`docker ps`{{execute}}

Gone!

Let's run another container up...

`docker run -d --name resplendent_redis redis`{{execute}}

But this time it **fails** - why is this?

Let's rerun the docker ps command but this time an an extra argument _-a_

`docker ps -a`{{execute}}

The _-a_ argument will report on **all** containers, those that are running AND those that have exited.

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

