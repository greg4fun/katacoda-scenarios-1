### Networks 
https://docs.docker.com/network/

By running "ip a" (short for ip show addr) we can see the current network devices configured on the Docker host:

`ip a`{{execute}}

We can view the available docker networks with the following command:

`docker network list`{{execute}}

Here we can see the current networks bridge, host and none.

* **Bridge** - The default. Enable containers on the same Docker host to communicate with each other.
* **Host** -  Removes network isolation between the container and the Docker host and uses the host’s networking directly.
* **None** - No networking

Others Network Types:

* **Overlay** - Connects multiple Docker daemons together. You can also use overlay networks to enable communication between standalone containers on different Docker daemons.
* **Macvlan** - Allows you to assign a MAC address to a container to make it appear as a physical device on the network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network. 

Creating a docker network called "my-network":

`docker network create my-network`{{execute}}

Rerunning the network list shows our new network:

`docker network list`{{execute}}

And by rerunning "ip a" we can see that new network devices have appeared:

`ip a`{{execute}}

Inspecting the new network:

`docker inspect my-network`

You can access a network when starting a container with the run command and the --net option:

`docker run -d --rm --name=redis --net=my-network redis`{{execute}}

* Q Should I try and show the network form inside a container bearing in mind ip does not exist...?

`docker run -it --rm --name=busybox --net=my-network busybox sh`{{execute}}

* Show how to connect/disconnect containers?

`docker run -it --rm --name=busybox_2 busybox sh`{{execute T2}}

`ip a`{{execute}}

`docker run -d --rm --name=busybox_2 busybox sh -c "sleep 600"`{{execute T2}}

`docker ps -a`{{execute T1}}`

`ip a`{{execute}}

`docker network connect my-network busybox_2`{{execute}}

* How can I tell if a container is attached to a particular network?

Checking which networks a container is connected to:

`docker inspect redis -f "{{json .NetworkSettings.Networks}}"`{{execute}}

Checking which containers are connected to a network:

`docker inspect my-network -f "{{json .Container}}"`{{execute}}

`docker network create my-network2`{{execute}}

`docker run --rm --name testing_networks -d -p 3000:3000 --net=my-network2 katacoda/redis-node-docker-example`{{execute}}

`curl docker:3000`{{execute}}

`docker network connect my-network redis`{{execute}}

`docker network list`{{execute}}

To examine the new network in detail:

`docker network inspect my-network`{{execute}}

# Network Aliases

`docker network create my-network3`{{execute}}

Assign the db alias to the redis instance on network: my-network3

`docker network connect --alias db my-network3 redis`{{execute}}

`docker run --net=my-network3 alpine ping -c1 db`{{execute}}

[Question: How do I identify the network aliases?]
Answer: docker inspect redis

                "my-network3": {
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "db",
                        "3362045b2d80"
                    ],

### Volumes 

### Data Containers
