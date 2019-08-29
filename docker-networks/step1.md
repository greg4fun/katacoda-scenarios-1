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

-----

### What is a Docker Network?
In order for containers to communicate with each other and the outside world they require access to a network.

Docker networks abstract and standardise the more complicated details involved in network connectivity and thus greatly simplify the connecting containers together.

To achieve this, Docker's network subsystem uses pluggable network drivers. By default Docker provides the **bridge**, **overlay** and **macvlan** drivers.

# Default Networks
Out of the box and installation of Docker Engine will provide three default networks.

We can view the available docker networks with the following command:

`docker network list`{{execute}}

Here we can see the current networks bridge, host and none.

* **Bridge** - The default. Enable containers on the same Docker host to communicate with each other.
* **Host** -  Removes network isolation between the container and the Docker host and uses the host’s networking directly.
* **None** - No networking

Others Network Types:

* **Overlay** - Enables communication between standalone containers on different Docker daemons. This is used by Docker Swarm clients.
* **Macvlan** - Allows you to assign a MAC address to a container to make it appear as a physical device on the network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network. 
* **Network Plugins** - Uses the Docker Network API to allow a wide range of other network technologies to be used to extend the basic Docker functionality.

The default bridge driver manifests itself as the **docker0** interface.

We can see this by running "ip a" (short for "ip addr show"):

`ip a`{{execute}}

Currently there are no containers running on so we only see the default network devices.

Starting a new container:

`docker run -d --rm --name uber_ubuntu ubuntu sh -c "sleep 900"`{{execute}}

And rerunning ip -a:

`ip a`{{execute}}

Notice that a virtual interface belonging to the container has appeared and the docker0 bridge has changed to a state of UP.

-----

# Creating our first network

Docker networks are created and managed with the "docker network" command.

In the following example we create a network called "red-network" but we provide no other options:

`docker network create red-network`{{execute}}

If we now rerun the _network list_ we can see our new network:

`docker network list`{{execute}}

Notice that by default docker has created it as a _bridge_ network.

By rerunning "ip a" we can see that a new network device has appeared:

`ip a`{{execute}}

This is a bridge device to link the virtual interface from the container to the physical network on Docker host.

[DIAGRAM]

# Inspecting the new network

As with other Docker objects you can use the "docker inspect" command to find out more detailed information.

`docker inspect red-network`{{execute}}

By default a container a loopback interface and an interface to the default bridge network.

`docker run --rm busybox sh -c "ip a"`{{execute T2}}

You can access a specific network when starting a container with the run command and the --net option, here we connect to the red-network we created earlier:

`docker run -d --rm --net=red-network --name=bright_busybox busybox sh -c "sleep 900"`{{execute T2}}

How does this network appear from inside the container? 

We can execute the "ip a" command in the bright_busybox container:

`docker exec bright_busybox ip a`{{execute T2}}

Comparing ip output to the Docker Host's network interfaces:

`ip a`{{execute T1}}

This shows us that the bright_busybox container is connected to our "red-network" network, see that the ip address ranges match.

- Checking which networks a container is connected to:

`docker inspect bright_busybox -f "{{json .NetworkSettings.Networks}}"|jq`{{execute}}

Here we can see that bright_busybox is on "red-network", as expected.

- Checking which containers are connected to a network:

`docker inspect red-network -f "{{json .Containers}}"|jq`{{execute}}

Here we can see that red-network only has one container connected and it is bright_busybox.
