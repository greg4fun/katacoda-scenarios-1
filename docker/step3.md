### Testing Our Container

Lets run a quick test against our container.

Here we start a new container called **my-redis-cli**.

We will use this container to run some test commands against **resplendent_redis**

`docker run -it --rm --name my-redis-cli goodsmileduck/redis-cli sh`{{execute T2}}

`redis-cli -h 172.18.0.2 -p 6379`{{execute}}

`set KUBEDOJO ROCKS!`{{execute}}

`get KUBEDOJO`{{execute}}

---

## Stopping Containers

The **stop** command will attempt to gracefully shutdown the container. 

If you dont care about shutting down gracefully you can use the **kill** option instead.

`docker stop resplendent_redis`{{execute T1}}

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

Rather than having to remember to tidy up you can add the **--rm** option which will automatically remove the container config when the process finishes.

`docker run --rm -d -p 6379:6379 --name resplendent_redis redis`{{execute}}

`docker ps -a`{{execute}}

`docker stop resplendent_redis`{{execute}}

`docker ps -a`{{execute}}

As you can see, the container no longer exists.

-----

# Exercise: Deploying Your First Docker Container

https://www.katacoda.com/courses/docker/deploying-first-container

---

