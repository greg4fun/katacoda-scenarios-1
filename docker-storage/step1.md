### Using Docker Volumes

Docker docs: https://docs.docker.com/storage/

Topics covered:
- What is a Docker Volume?
- How do I view the default volumes? [docker volume ls]
- How do I use the default volumes? [docker volume create/attach/detach]
- What are the different types of Docker Volumes?
- How do I view detailed info on a network [docker inspect]


"Docker has two options for containers to store files in the host machine, so that the files are persisted even after the container stops: volumes, and bind mounts. If you’re running Docker on Linux you can also use a tmpfs mount. If you’re running Docker on Windows you can also use a named pipe." 