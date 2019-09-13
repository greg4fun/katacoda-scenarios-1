# Connecting and disconnecting Containers from a Network

Let's create another Bridge network called "green-network", but this time specifying our choice of subnet and a range of ips:

`docker network create --subnet 172.20.0.0/16 --ip-range 172.20.240.0/20 green-network`{{execute T1}}

Our new green-network has now been created - let's check:

`docker network list`{{execute T1}}

Ok, so how do we connect to this new network?

If we want to connect a running container to the new network we used the **docker network connect** command:

`docker network connect green-network bright_busybox`{{execute T1}}

Let's check the new network from inside the __bright_busybox__ container:

`docker exec bright_busybox ip a`{{execute T2}}

And from the docker host we can see the new interface:

`ip a`{{execute T1}}

We can also disconnect a running container from a network:

`docker network disconnect green-network bright_busybox`{{execute T1}}

And to prove it we rerun the ip a commands on both container and docker host. Notice the veth has vanished.

`docker exec bright_busybox ip a`{{execute T2}}

`ip a`{{execute T1}}

-----

# Network Alias

An alias can be used to resolve the container by another name on a different network.

Assign the alias __jolly_giant__ to the bright_busybox instance on the green-network:

`docker network connect --alias jolly_giant green-network bright_busybox`{{execute}}

Now let's ping the alias __jolly_giant__:

First connect able_alpine to the green_network, as it is still languishing on the default bridged network.

`docker network connect green-network able_alpine`{{execute}}

Now execute a ping:

`docker exec able_alpine ping -c1 jolly_giant`{{execute}}

* How do I identify the network aliases?

`docker inspect bright_busybox -f '{{json .NetworkSettings.Networks}}' | jq`{{execute}}

----
