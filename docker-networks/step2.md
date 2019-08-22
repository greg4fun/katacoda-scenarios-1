# Connecting and disconnecting Containers from a Network

Let's create another Bridge network, but this time add more customisations:

`docker network create --subnet 172.20.0.0/16 --ip-range 172.20.240.0/20 blue-network`{{execute T1}}

`docker network list`{{execute T1}}

`docker network connect blue-network bright_busybox`{{execute T1}}

`docker exec bright_busybox ip a`{{execute T2}}

`ip a`{{execute T1}}

`docker network disconnect blue-network bright_busybox`{{execute T1}}

`docker exec bright_busybox ip a`{{execute T2}}

`ip a`{{execute T1}}

# Connecting and disconnecting Containers from a Network

How can we dynamically connect and disconnect containers from a network?

Let's create a new network - we'll call this one **my-network3**.

`docker network create my-network3`{{execute T1}}

OK, so we now have another network, let's see them all:

`docker network list`{{execute T1}}

`docker run --rm --name reliable_redis -d -p 3000:3000 --net=my-network2 katacoda/redis-node-docker-example`{{execute T1}}

`docker run --rm -d alpine curl docker:3000`{{execute}}

`curl docker:3000`{{execute}}

`docker network connect my-network resplendent_redis`{{execute}}

`docker network list`{{execute}}

To examine the new network in detail:

`docker network inspect my-network`{{execute}}

`docker network create my-network3`{{execute}}

# Network Aliases

An alias can be used to resolve the container by another name on a different network.

Assign the db alias to the redis instance on network: green-network

`docker network connect --alias db green-network redis`{{execute}}

`docker run --net=green-network alpine ping -c1 db`{{execute}}

* How do I identify the network aliases?

`docker inspect redis -f '{{json .NetworkSettings.Networks}}' | jq`{{execute}}

----

# DNS
