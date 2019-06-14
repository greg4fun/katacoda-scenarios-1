## Task Two Naming and Stopping Containers

The automatically generated names and container Ids are all well and good but what if you want to be able to refer to a container in a more predictable manner?

Fortunately Docker allows us to name containers ourselves.

By using the **--name** option we can

`docker run -d --name resplendent_redis redis`{{execute}}

Now when we user the "docker ps" command we can see there are 2 containers running. One of which is named, as expected **resplendent_redis**.

`docker ps`{{execute}}

We can now interact with the container using a predictable name.

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

Incidentally, you may wish to create a container with out starting it; a use case for this is data containers which we will learn about later.

To **create** a container:

`docker create --name resplendent_redis redis`{{execute}}

`docker ps -a`{{execute}}

You can start and stop this with the commands you have just learnt.

When you have finished playing stop and remove the config.

I don't know about you, but it's a bit of a pain to have to keep remembering to remove the container after it has exited.

Fortunately there is an option for docker run to help **--rm**. This option tells docker to remove the config when the container exits.

Let's test it...

`docker run --rm -d --name resplendent_redis redis`{{execute}}





