## Task Two Naming and Stopping Containers

The automatically generated names and container Ids are all well and good but what if you want to be able to refer to a container in a more predictable manner?

Fortunately Docker allows us to name containers ourselves.

By using the **--name** option we can

`docker run -d --name Resplendent_Redis redis`{{execute}}