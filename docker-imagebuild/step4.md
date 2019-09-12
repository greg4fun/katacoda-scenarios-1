# Dockerfile Instructions
A high level overview of some of the more frequently used Dockerfile instructions:

# FROM
  ```FROM image; [AS name]```
  
  Initialises a new build stage based on the image in the FROM instruction.
  This instruction must always be present on a Dockerfile.

  "FROM scratch" in a Dockerfile indicates that the special "scratch" docker image should be used. This image is completely empty!
  This is used when creating __base images__ from the ground up. Base images are used as parent images for further image builds.

  Optionally a name can be given to a new build stage by adding AS name to the FROM instruction.
  The name can be used in subsequent FROM and COPY --from=name|index; instructions to refer to the image built in this stage.

# RUN 
  Execute commands inside of your image which get executed at build time and get written into the image as a new layer.
  
  ```RUN command (shell form)```  
  
  Example: ```RUN yum install -y httpd```
  
  ```RUN ["executable", "param1", "param2"] (exec form)```
  
  Example: ```RUN ["yum", "install", "-y", "httpd"]```
    
  Unlike the shell form the exec for does NOT invoke a command shell and so does not (normally) parse variables.
  
  The cache for RUN instructions is not invalidated automatically during the next build, so the cache for an instruction like ```RUN apt-get dist-upgrade -y``` will be reused during the next build.
  
  The cache can be invalidated by using the --no-cache flag, for example docker build --no-cache.
  
  
# ADD
  ```ADD [--chown=user:group] src... dest```
  
  The ADD instruction copies new files, directories or remote file URLs from src and adds them to the filesystem of the image at the path dest.

  Multiple src resources may be specified but if they are files or directories, their paths are interpreted as relative to the source of the context of the build.

  All new files and directories are created with a UID and GID of 0, unless the optional --chown flag specifies a given username, groupname, or UID/GID combination to request specific ownership of the content added.
  
  ADD does not support authentication.

# COPY
  ```COPY [--chown=user:group] src... dest```

  The COPY instruction copies new files or directories from src and adds them to the filesystem of the container at the path dest.

  Multiple src resources may be specified but the paths of files and directories will be interpreted as relative to the source of the context of the build.

  All new files and directories are created with a UID and GID of 0, unless the optional --chown flag specifies a given username, groupname, or UID/GID combination to request specific ownership of the copied content.

  Although ADD and COPY are functionally similar, generally speaking, COPY is preferred.

- CMD Defines a default command to run when your container starts.

  The CMD instruction should be used to run the software contained by your image, along with any arguments. 
  CMD should almost always be used in the form of CMD ["executable", "param1", "param2"…]

  For example: ```CMD ["apache2","-DFOREGROUND"]```
  
  CMD is also used to set default arguments for the ENTRYPOINT instruction (using JSON array format)


# ENTRYPOINT
  An ENTRYPOINT allows you to configure a container that will run as an executable.

  ENTRYPOINT has two forms:

  ```ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)
  ENTRYPOINT command param1 param2 (shell form)```

  The exec form of ENTRYPOINT allows you to set commands and parameters and then use either form of CMD to set additional parameters that are more likely to be changed. 
  
  ENTRYPOINT arguments are always used, while CMD ones can be overwritten by command line arguments provided when Docker container runs. 

  For example:

  ```ENTRYPOINT ["/bin/echo", "Hello"]
  CMD ["World"]```
  
  Then when the container runs as docker run -it ```image``` will produce output:

  ```Hello World```

  But when run as: ```docker run -it <image> Dojo```
  
  Will result in:

  ```Hello Dojo```

  Only the last ENTRYPOINT instruction in the Dockerfile will have an effect.

# WORKDIR
  ```WORKDIR /path/to/workdir```
  
  The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile. 
  
  If the WORKDIR doesn’t exist, it will be created even if it’s not used in any subsequent Dockerfile instruction.

  The WORKDIR instruction can be used multiple times in a Dockerfile. If a relative path is provided, it will be relative to the path of the previous WORKDIR instruction
  

# ONBUILD 
  Executes commands only when the image is used as a base image.


# USER
  ```USER <user>[:<group>] or
  USER <UID>[:<GID>]```
  
  The USER instruction sets the user name (or UID) and optionally the user group (or GID) to use when running the image and for any RUN, CMD and ENTRYPOINT instructions that follow it in the Dockerfile.
  
  The default user is UID is 0 (root) and and the default group is 0 (root). 
  
  NOTE: If the UID or GID being selected does not exist the root will be used.

# EXPOSE
  The EXPOSE instruction indicates the ports on which a container listens for connections. 
  
  For example, an image containing the Apache web server would normally use ```EXPOSE 80```

# ENV
  To make new software easier to run, you can use ENV to update the PATH environment variable for the software your container installs. 
  
  For example, ```ENV PATH /usr/local/nginx/bin:$PATH``` ensures that ```CMD ["nginx"]``` just works.
  
# LABEL
  The LABEL instruction adds metadata to an image. A LABEL is a key-value pair. To include spaces within a LABEL value, use quotes and backslashes as you would in command-line parsing.
  
  For example: ```LABEL maintainer="Kube Dojo"```
  
# VOLUME  
  The VOLUME instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.
  
  For example:
  
  ```FROM ubuntu
  RUN mkdir /myvol
  RUN echo "hello world" > /myvol/greeting
  VOLUME /myvol```

  This Dockerfile results in an image that causes docker run to create a new mount point at /myvol and copy the greeting file into the newly created volume.
