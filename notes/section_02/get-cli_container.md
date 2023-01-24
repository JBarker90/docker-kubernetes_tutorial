# Getting a Command Prompt in a Container

### 1. Getting a Command Prompt:

- Sometimes you might want to open a command prompt for a running container. 
- This can be done with `docker exec`

```
docker exec -it <container id> <terminal env>
```

Example: Using the already running Redis instance, you can grab the container ID and then get an `sh` command prompt

```
jonathan@dockerhost-02:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS      NAMESa6f12c44cca8   redis     "docker-entrypoint.s…"   12 seconds ago   Up 9 seconds   6379/tcp   charming_tu

jonathan@dockerhost-02:~$ docker exec -it a6f12c44cca8 bash
root@a6f12c44cca8:/data# id
uid=0(root) gid=0(root) groups=0(root)
```

### 2. Starting a Container with a Shell:

- Rather than running a container and then later getting a shell, you can get a command prompt immediately when starting a conatiner
- This can be done with `docker run`

```
docker run -it <container image> <shell env>
```

Example: Getting `sh` environment when initially running busybox

```
jonathan@dockerhost-02:~$ docker run -it busybox sh
/ # id
uid=0(root) gid=0(root) groups=0(root),10(wheel)
```