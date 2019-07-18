### Networks 

`docker network create backend-network`{{execute}}

`docker run -d --name=redis --net=backend-network redis`{{execute}}

`docker network connect backend-network redis`{{execute}}

`docker run --rm --name testing_networks -d -p 3000:3000 --net=frontend-network katacoda/redis-node-docker-example`{{execute}}

`curl docker:3000`{{execute}}

`docker network list`{{execute}}

To examine the network in detail:
docker network inspect <network name>

`docker network inspect backen-network`{{execute}}

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
