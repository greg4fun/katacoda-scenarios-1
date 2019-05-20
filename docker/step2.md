## Task Two Inspecting the Container

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