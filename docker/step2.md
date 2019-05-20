## Task Two Inspecting a Running Container

Now that we have been returned to the command line we can use docker ps to list running containers. Try this:

`docker ps`{{execute}} 

Docker ps is based on a well known Unix/Linux command called 'ps', which stands for Process Status.

**CONTAINER ID** Only the first 12 digits of the hash are shown in this column. This is sufficient to ensure a unique ID.

**IMAGE** is the name of the image used to instantiate the container.

**COMMAND** is the command being run by the container.

**CREATED** is when the container configuration on disk was created.

**STATUS** is the current status of the container.

**PORTS** describes the available ports or port range.

**NAMES** the name of the container as provided or randomly set by docker.

# Inspect

Let's _inspect_ our container.

You have 2 choices here, you can either use the CONTAINER ID or the CONTAINER NAME.

Your commands will be similar to the following: 

`docker inspect distracted_nightingale`{{copy}}

`docker inspect 642736e3e640`{{copy}}

Note - make sure you replace the CONTAINER ID or NAME with the details from _your_ container!

The inspect command returns a very detailed description of your container, including:

Id, State, Image info, Volume info, Resource allocation, Mounts and Network settings.

# Logs

Let's look at the _logs_

As before you can use either the CONTAINER ID or NAME.

`docker logs distracted_nightingale`{{copy}}

Remember the output we saw on the screen when the container was started without the -d option?

Now we are running in detached mode this output has gone into the container's log.