# Connecting and disconnecting Containers from a Network

Let's create another Bridge network, but this time add more customisations:

`docker network create --subnet 172.20.0.0/16 --ip-range 172.20.240.0/20 custom-bridge-network`{{execute T1}}

`docker network list`{{execute T1}}

`docker network connect cutsome-bridge-network bright_busybox`{{execute T1}}

`docker exec bright_busybox ip a`{{execute T2}}

`ip a`{{execute T1}}

`docker network disconnect custome-bridge-network bright_busybox`{{execute T1}}

`docker exec bright_busybox ip a`{{execute T2}}

`ip a`{{execute T1}}

# Connecting and disconnecting Containers from a Network

How can we dynamically connect and disconnect containers from a network?

Let's create a new network - we'll call this one **my-network3**.

`docker network create my-network3`{{execute T1}}

OK, so we now have another network, let's see them all:

`docker network list`{{execute T1}}

`docker run --rm --name reliable_redis -d -p 3000:3000 --net=my-network2 katacoda/redis-node-docker-example`{{execute T1}}

`curl docker:3000`{{execute}}

`docker network connect my-network resplendent_redis`{{execute}}

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
