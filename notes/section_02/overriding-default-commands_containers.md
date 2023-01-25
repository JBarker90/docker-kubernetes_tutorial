# Overriding Default Commands

### 1. To run a Docker container, you can use `docker run`

```
docker run <image_name>  
```

Example:

```
docker run hello-world
```

```
jonathan@dockerhost-02:~$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### 2. While running a Docker image, you can use override commands

- This is essentially executing a specific command after the image runs.

```
docker run <image_name> <command override>
```

Example:

```
jonathan@dockerhost-02:~$ docker run busybox ls
Unable to find image 'busybox:latest' locally
latest: Pulling from library/busybox
2123501b93d4: Pull complete
Digest: sha256:05a79c7279f71f86a2a0d05eb72fcb56ea36139150f0a75cd87e80a4272e4e39
Status: Downloaded newer image for busybox:latest
bin
dev
etc
home
lib
lib64
proc
root
sys
tmp
usr
var
```

NOTE: After busybox starts up, it runs `ls` on the root directory of the new container. 