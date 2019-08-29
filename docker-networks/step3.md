# DNS

- How does DNS work with a Docker Network?

Run a redis container instance:

`docker run -d -p 6379:6379 --name resplendent_redis redis`{{execute T1}}

And run another container with the redis-cli:

`docker run -it --rm --name my-redis-cli goodsmileduck/redis-cli sh`{{execute T2}}

Attempt to connect with:

`redis-cli -h resplendent_redis -p 6379`{{execute T2}}

- This fails - why?

`exit`{{execute T2}}

`cat /etc/resolv.conf`{{execute T2}}

`docker network list`{{execute T1}}

`docker network connect red-network resplendent_redis`{{execute T1}}

`docker network connect red-network my-redis-cli`{{execute T1}}

`docker inspect red-network -f "{{json .Containers}}" | jq`{{execute T1}}

`cat /etc/resolv.conf`{{execute T2}}

`redis-cli -h resplendent_redis -p 6379`{{execute T2}}

`set KUBE DOJO`{{execute T2}}

`get KUBE`{{execute T2}}
