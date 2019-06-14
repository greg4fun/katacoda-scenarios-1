### Running a container

Let's run our first container:

`docker run -d -p 6379:6379 --name resplendent_redis redis`{{execute}} 

The **run** option creates a container configuration on the filesystem under **/var/lib/docker** on the Docker Master and then starts the container.

The **-d** option ensures the container runs in **detached** mode. This means the container runs without capturing the prompt.

The **--name resplendent_redis** option allows me to a predictable name to the container. This is optional but useful.

The last option **redis** is the name of the image.

The **hash string** returned is the unique identifying ID for the container you have just started.

### Listing containers

To view a list of running containers we use the ps option.

`docker ps -a`{{execute}} 

Docker ps is based on a well known Unix/Linux command called 'ps', which stands for Process Status.

The **-a** option will show _all_ containers both running and exited.

**CONTAINER ID** Only the first 12 digits of the hash are shown in this column. This is sufficient to ensure a unique ID.

**IMAGE** is the name of the image used to instantiate the container.

**COMMAND** is the command being run by the container.

**CREATED** is when the container configuration on disk was created.

**STATUS** is the current status of the container.

**PORTS** describes the available ports or port range.

**NAMES** the name of the container as provided or randomly set by docker.

Let's _inspect_ our container.

`docker inspect resplendent_redis`{{execute}}

The inspect command returns a very detailed description of your container, including:

Id, State, Image info, Volume info, Resource allocation, Mounts and Network settings.

### Stats

`docker stats`{{execute}}

### Top

`docker top resplendent_redis`{{execute}}

### Logs

Let's look at the _logs_

`docker logs resplendent_redis`{{execute}}
