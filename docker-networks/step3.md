# DNS

- How does DNS work with a Docker Network?

Run a redis container instance:

`docker run -d -p 6379:6379 --name resplendent_redis redis`{{execute T1}}

And run another container with the redis-cli:

`docker run -it --rm --name my-redis-cli goodsmileduck/redis-cli sh`{{execute T2}}

Connect to the redis service on resplendent_redis from my-redis-cli:

`redis-cli -h resplendent_redis -p 6379`{{execute T2}}

- What happens - why?

Exit out of the redis-cli:

`exit`{{execute T2}}

And let's have a look in the resolve.conf file of the container, my-redis-cli:

`cat /etc/resolv.conf`{{execute T2}}

Now remember, we did not assign a network when we created resplendent_redis and my-redis-cli.

This means the containers were connected to the default bridged network and this network sets the nameserver to 8.8.8.8 (Google).

So let's now connect our two containers to one of our custom bridge networks - the **red-network**:

`docker network connect red-network resplendent_redis`{{execute T1}}

`docker network connect red-network my-redis-cli`{{execute T1}}

So now we can see them on the red-network:

`docker inspect red-network -f "{{json .Containers}}" | jq`{{execute T1}}

Now when we look at the resolv.conf file we can see that the nameserver has changed to be controlled by the Docker embedded DNS Service.

`cat /etc/resolv.conf`{{execute T2}}

Attempting to connect to the redis service on resplendent_redis from my-redis-cli again:

`redis-cli -h resplendent_redis -p 6379`{{execute T2}}

`set KUBE DOJO`{{execute T2}}

`get KUBE`{{execute T2}}

Success!
