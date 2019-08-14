### Using Docker Networks

Docker docs: https://docs.docker.com/network/

Topics covered:
- What is a Docker Network?
- What are the default networks? [docker network list]
- How do I use the default networks? [docker network create/attach/detach]
- What are the different types of Docker Network?
- How do I view detailed info on a network [docker inspect]
- How can I see which networks my container is connected to?
- How can I see which containers are connected to a specific network?

### What is a Docker Network?
In order for containers to communicate with each other and the outside world they require access to a network.

Docker networks abstract and standardise the more complicated details involved in network connectivity and thus greatly simplify the connecting containers together.

To achieve this, Docker's network subsystem uses pluggable network drivers. By default Docker provides the **bridge** and **overlay** drivers.

# Default Networks
Out of the box and installation of Docker Engine will provide three default networks.

We can view the available docker networks with the following command:

`docker network list`{{execute}}

Here we can see the current networks bridge, host and none.

* **Bridge** - The default. Enable containers on the same Docker host to communicate with each other.
* **Host** -  Removes network isolation between the container and the Docker host and uses the host’s networking directly.
* **None** - No networking

Others Network Types:

* **Overlay** - Connects multiple Docker daemons together. You can also use overlay networks to enable communication between standalone containers on different Docker daemons. This is used by Docker Swarm clients.
* **Macvlan** - Allows you to assign a MAC address to a container to make it appear as a physical device on the network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network. 

The default bridge driver manifests itself as the **docker0** interface.

We can see this by running "ip a" (short for "ip show addr"):

`ip a`{{execute}}

-----

# Creating our first network

Docker networks are created and managed with the "docker network" command.

In the following example we create a network called "my-network" but we provide no other options:

`docker network create my-network`{{execute}}

If we now rerun the _network list_ we can see our new network:

`docker network list`{{execute}}

Notice that by default docker has created it as a _bridge_ network.

By rerunning "ip a" we can see that a new network device have appeared:

`ip a`{{execute}}

This is a bridge device to link the virtual interface from the container to the physical network on Docker host.

# Inspecting the new network

As with other Docker objects you can use the "docker inspect" command to find out more detailed information.

`docker inspect my-network`{{execute}}

By default a container a loopback interface and an interface to the default bridge network.

`docker run --rm busybox sh -c "ip a"`{{execute T2}}

You can access a specific network when starting a container with the run command and the --net option, here we connect to the my_network we created earlier:

`docker run -d --rm --net=my-network --name=bright_busybox busybox sh -c "sleep 900"`{{execute T2}}

How does this network appear from inside the container? 

We can execute the "ip a" command in the bright_busybox container:

`docker exec bright_busybox ip a`{{execute T2}}

Comparing ip output to the Docker Host's network interfaces:

`ip a`{{execute T1}}

THis shows us that the bright_busybox container is connected to our "my-network" network, see that the ip address ranges match.

- Checking which networks a container is connected to:

`docker inspect bright_busybox -f "{{json .NetworkSettings.Networks}}"`{{execute}}

Here we can see that bright_busybox is on "my-network", as expected.

- Checking which containers are connected to a network:

`docker inspect my-network -f "{{json .Containers}}"`{{execute}}

Here we can see that my-network only has one container connected and it is bright_busybox.

# Attaching and detaching Containers from a Network

We'll need another network:

`docker network create my-network2`{{execute T1}}

`docker network create --subnet 172.20.0.0/16 --ip-range 172.20.240.0/20 multi-host-network`{{execute T1}}

`docker network list`{{execute T1}}

`docker network attach bright_busybox multi-host-network`{{execute T1}}

`docker exec bright_busybox ip a`{{execute T2}}

`ip a`{{execute T1}}

`docker network dettach bright_busybox multi-host-network`{{execute T1}}

`docker exec bright_busybox ip a`{{execute T2}}

`ip a`{{execute T1}}

# Attaching and detaching Containers from a Network

How can we dynamically attach and detach containers from a network?

Let's create a new network - we'll call this one **my-network3**.

`docker network create my-network3`{{execute T1}}

OK, so we now have another network, let's see them all:

`docker network list`{{execute T1}}

`docker run --rm --name reliable_redis -d -p 3000:3000 --net=my-network2 katacoda/redis-node-docker-example`{{execute T1}}

`curl docker:3000`{{execute}}

`docker network connect my-network redis`{{execute}}

`docker network list`{{execute}}

To examine the new network in detail:

`docker network inspect my-network`{{execute}}

`docker network create my-network3`{{execute}}

# Network Aliases

An alias can be used to resolve the container by another name on a different network.

Assign the db alias to the redis instance on network: my-network3

`docker network connect --alias db my-network3 redis`{{execute}}

`docker run --net=my-network3 alpine ping -c1 db`{{execute}}

[Question: How do I identify the network aliases?]
Answer: docker inspect redis

docker inspect redis -f '{{json .NetworkSettings.Networks}}' | jq

                "my-network3": {
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "db",
                        "3362045b2d80"
                    ],
