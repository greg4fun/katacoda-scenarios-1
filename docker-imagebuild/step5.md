# Best Practices

# Build ephemeral containers

It should be possible for the container to be stopped and destroyed, then rebuilt and replaced with the minimum set-up and configuration.

---
#Keep an eye on the size on the build context

All recursive contents of files and directories in the in the "docker build" current directory are sent to the Docker daemon as the build contex.

Including files that are not necessary for building an image results in a larger build context and larger image size.

This can increase the time to build the image, time to pull and push it, and the container runtime size. To see how big your build context is, look for a message like this when building your Dockerfile:

```Sending build context to Docker daemon  187.8MB```

---
# Exclude with .dockerignore

To exclude files not relevant to the build (without restructuring your source repository) use a .dockerignore file. 

This file supports exclusion patterns similar to .gitignore files. 

---
# Do not install unnecessary packages

To reduce complexity, dependencies, file sizes, and build times, avoid installing extra or unnecessary packages just because they might be “nice to have.” 

For example, you don’t need to include a text editor in a database image.

---
# Do not use --privileged containers

Privileged containers are defined as any container where the container uid 0 is mapped to the host’s uid 0.

In such containers, protection of the host and prevention of escape is entirely done through Mandatory Access Control (apparmor, selinux), seccomp filters, dropping of capabilities and namespaces.

It is almost like granting root privileges on the underlying docker host to the container.

Read this article for much much more detal on why this is bad: https://brauner.github.io/2019/02/12/privileged-containers.html

---
# A Single Application per container

Each container should have only one purpose. Decoupling applications into multiple containers makes it easier to scale horizontally and reuse containers. 

For instance, a web application stack might consist of three separate containers, each with its own unique image, to manage the web application, database, and an in-memory cache in a decoupled manner.

As a rule of thumb consider limiting to one process per container - where practical.

---
# Use Fixed Tags rather than latest

A Docker image can have multiple tags, which are variants of the same images. 

The most common tag is latest, which represents the latest version of the image. Image tags are not immutable, and the author of the images can publish the same tag multiple times.

This means that the base image for your Docker file might change between builds. This could result in inconsistent behavior because of changes made to the base image.

Prefer the most specific tag available. If the image has multiple tags, such as :8 and :8.0.1 or even :8.0.1-alpine, prefer the latter, as it is the most specific image reference. 

Keep in mind that when pinning a specific tag, it might be deleted eventually.

# Use the Least privileged user

---
# Don’t leak sensitive information to Docker images

Ensure that you do not accidentally copy files with secrets into the image, even if they are deleted later they will remain in that image and all child images built from this image. They can be retrieved and accessed!

---
# Use Multi-stage builds

Multi-stage builds allow you to drastically reduce the size of your final image, without struggling to reduce the number of intermediate layers and files.

Because an image is built during the final stage of the build process, you can minimize image layers by leveraging build cache.

For example, if your build contains several layers, you can order them from the less frequently changed (to ensure the build cache is reusable) to the more frequently changed:

- Install tools you need to build your application

- Install or update library dependencies

- Generate your application

---
# Use Labels

Image labels provide metadata for the image you’re building. This help users understand how to use the image easily. The most common label is “maintainer”, which specifies the email address and the name of the person maintaining this image. Add metadata with the following LABEL command:

```LABEL maintainer="kube@dojo.com"```

In addition to a maintainer contact, add any metadata that is important to you.

---
# Prefer COPY over ADD

Although ADD and COPY are functionally similar, generally speaking, COPY is preferred.

That’s because it’s more transparent than ADD. COPY only supports the basic copying of local files into the container, while ADD has some features (like local-only tar extraction and remote URL support) that are not immediately obvious. 

The best use for ADD is local tar file auto-extraction into the image, as in ADD rootfs.tar.xz /.

---
# Leverage the build cache

When building an image, Docker steps through the instructions in your Dockerfile, executing each in the order specified. As each instruction is examined, Docker looks for an existing image in its cache that it can reuse, rather than creating a new (duplicate) image.

Once the cache is invalidated, all subsequent Dockerfile commands generate new images and the cache is not used.
---

