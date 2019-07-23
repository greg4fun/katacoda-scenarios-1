### Networks 
https://docs.docker.com/network/

We can view the available docker networks with the following command:

`docker network list`{{execute}}

Here we can see the current networks bridge, host and none.

* **Bridge** - The default. Enable containers on the same Docker host to communicate with each other.
* **Host** -  Removes network isolation between the container and the Docker host and uses the host’s networking directly.
* **None** - No networking

Others Network Types:

* **Overlay** - Connects multiple Docker daemons together. You can also use overlay networks to enable communication between standalone containers on different Docker daemons.
* **Macvlan** - Allows you to assign a MAC address to a container to make it appear as a physical device on the network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network. 

Creating a docker network called "backend-network":

`docker network create backend-network`{{execute}}

Rerunning the network list shows our new network:

`docker network list`{{execute}}

You can access a network when starting a container with the run command and the --net option:

`docker run -d --rm --name=redis --net=backend-network redis`{{execute}}

* Q Should I try and show the network form inside a container bearing in mind ip does not exist...?

* Show how to connect/disconnect containers?

* How can I tell if a container is attached to a particular network?

`docker network create frontend-network`{{execute}}

`docker run --rm --name testing_networks -d -p 3000:3000 --net=frontend-network katacoda/redis-node-docker-example`{{execute}}

`curl docker:3000`{{execute}}

`docker network connect backend-network redis`{{execute}}

`docker network list`{{execute}}

To examine the network in detail:
docker network inspect <network name>

`docker network inspect backend-network`{{execute}}

# Network Aliases

`docker network create frontend-network2`{{execute}}

Assign the db alias to the redis instance on network: frontend-network2

`docker network connect --alias db frontend-network2 redis`{{execute}}

`docker run --net=frontend-network2 alpine ping -c1 db`{{execute}}

[Question: How do I identify the network aliases?]
Answer: docker inspect redis

                "frontend-network2": {
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "db",
                        "3362045b2d80"
                    ],

### Volumes 

### Data Containers
