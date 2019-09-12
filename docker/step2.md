### Running a container

Let's run our first container:

`docker run -d -p 6379:6379 --name resplendent_redis redis`{{execute}} 

The **run** option creates a container configuration on the filesystem under **/var/lib/docker** on the Docker Master and then starts the container.

The **-d** option ensures the container runs in **detached** mode. This means the container runs without capturing the prompt.

The **-p** option maps the docker host's port to the container's port.

The **--name resplendent_redis** option allows me to call the container a predictable name. This is optional but useful.

The last option, **redis** is the name of the image.

The **hash string** returned is the unique identifying ID for the container you have just started.

---

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

Last time we ran docker _inspect_ we inspected an image. Now let's _inspect_ our container:

`docker inspect resplendent_redis`{{execute}}

The inspect command returns a very detailed description of your container, including:

Id, State, Image info, Volume info, Resource allocation, Mounts and Network settings.

----

### Stats

The current performance stats per container can be viewed:

`docker stats`{{execute}}

Exit by pressing _Ctrl+C_ or click the command:

`echo "Sending Ctrl+C"`{{execute interrupt}}

----

### Executing Commands in a Running Container

In this use case we are have a running container called **resplendent_redis**.

I want to run the **whoami** command in that container so I can see what userid is active.

`docker exec resplendent_redis whoami`{{execute}}

So we can see in the output that we are running as root.

----

### Accessing an interactive shell

It is also possible to start a container with a shell as a command. This will start an interactive container which can be very useful for debugging.

`docker run -it ubuntu bash`{{execute}}

Note that shell types will vary by image, either due to distribution or author choice.

Bourne shell is most common (sh).
Bourne Again shell is often provided (bash).
Alpine uses ash.

----

### Top - Display Container Processes

`docker top resplendent_redis`{{execute}}

Despite the whoami command stating that we are running as root, here the PID is owned by user 999.

This is a security feature that means that should the user somehow execute commands outside the container they will not have root privileges.

----

### Logs

Let's look at the _logs_

`docker logs resplendent_redis`{{execute}}

----

### Inspect Formatting

Docker supports formatting of output on the GO template format.

Let's grab the container's IPAddress, we'll need that for the next section.

`docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' resplendent_redis`{{execute}}

----
