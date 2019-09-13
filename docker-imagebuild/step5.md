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

To exclude files not relevant to the build (without restructuring your source repository) use a .dockerignore file. 

This file supports exclusion patterns similar to .gitignore files. 

---
#Do not install unnecessary packages

To reduce complexity, dependencies, file sizes, and build times, avoid installing extra or unnecessary packages just because they might be “nice to have.” 

For example, you don’t need to include a text editor in a database image.

---
#Do not use --privileged containers

Read this article for much much more detal on why this is bad: https://brauner.github.io/2019/02/12/privileged-containers.html

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
#Leverage the build cache

---

