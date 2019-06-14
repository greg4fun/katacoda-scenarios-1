Welcome to this BFest Docker Demo.

Images are stored in Registries. By default Katacoda uses DockerHub however internally we currently use Quay.

How to search for an image on the Registry:

`docker search redis`{{execute}} 

In this example we are searching for the **redis** image.

Let's retrieve the image from the remote registy:

`docker pull redis`{{execute}}

By default this pulls the latest image version.

You can pull specific images by specifiying a tag after the image:

`docker pull redis:4`{{execute}}

This will pull the redis image associated with tag **5** usually this is a version number.

We can view our local images:

`docker images`{{execute}}

For more info about an image:

`docker inspect redis`{{execute}}
