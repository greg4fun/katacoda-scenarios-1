### Kubernetes Community Dojo for Docker Presentation
---  
 
### The Docker Environment

It is useful to understand the docker environment we are using.

The _docker version_ command provides version info for both the client (CLI) and the Server (Engine).

`docker version`{{execute}}

The _docker info_ command provides quite detailed information about the docker host.

`docker info`{{execute}}

### Images

Images are stored in Registries. By default Katacoda uses DockerHub however internally we currently use Quay (go/Quay).

We can search for an image in the registry using the _docker search_ command.

In this example we are searching for images that mention **redis**:

`docker search redis`{{execute}} 

We can also view our local images, if we have any:

`docker images`{{execute}}

These local images may be old so let's pull a newer image from the registry.

Let's retrieve the image from the remote registy:

`docker pull redis`{{execute}}

By default this pulls the _latest_ image version. The default tag is "latest". 

You can pull specific images by specifiying a tag after the image:

`docker pull redis:4`{{execute}}

This will pull the redis image associated with tag **4** usually this is a version number.

How did I know this tag existed? This is a bit more awkward. One option is to go to dockerhub:

[https://hub.docker.com/_/redis/?tab=tags](https://hub.docker.com/_/redis/?tab=tags)

and the other is to use the registry API:

`curl 'https://registry.hub.docker.com/v2/repositories/library/redis/tags/'|jq '."results"[]["name"]'`{{execute}}

View our local images again to see any changes:

`docker images`{{execute}}

For much more detailed info about an image:

`docker inspect redis`{{execute}}
