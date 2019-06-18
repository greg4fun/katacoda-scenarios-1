#Welcome to this BFest Docker Demo.

Images are stored in Registries. By default Katacoda uses DockerHub however internally we currently use Quay.

We can search for an image in the regist useing the _docker search_ command.

In this example we are searching for images that mention **redis**:

`docker search redis`{{execute}} 

Let's retrieve the image from the remote registy:

`docker pull redis`{{execute}}

By default this pulls the latest image version.

You can pull specific images by specifiying a tag after the image:

`docker pull redis:4`{{execute}}

This will pull the redis image associated with tag **4** usually this is a version number.

We can view our local images:

`docker images`{{execute}}

For more info about an image:

`docker inspect redis`{{execute}}
