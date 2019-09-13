# Best Practices

#Build ephemeral containers

It should be possible for the container to be stopped and destroyed, then rebuilt and replaced with the minimum set-up and configuration.

---
#Keep an eye on the size on the build context

All recursive contents of files and directories in the in the "docker build" current directory are sent to the Docker daemon as the build contex.

Including files that are not necessary for building an image results in a larger build context and larger image size.

This can increase the time to build the image, time to pull and push it, and the container runtime size. To see how big your build context is, look for a message like this when building your Dockerfile:

```Sending build context to Docker daemon  187.8MB```

---
#Exclude with .dockerignore

---
#Do not install unnecessary packages

---
#Do not use --privileged containers

---
#A Single Application per container

---
#Use meaningful Tags

---
#Use the Least privileged user

---
#Don’t leak sensitive information to Docker images

---
#Use Multi-stage builds

---
#Use Labels

---
#Prefer COPY over ADD

---
