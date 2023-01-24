# Executing Commands in Running Containers

### 1. To Execute commands you can run `docker exec`

- This will allow you to execute commands in a running container

```
docker exec -it <container id> <command>
```

NOTE: The `-it` flags allow us to provide input to the container

Example: I ran a Redis instance using `docker run redis` and then executed redis-cli

```
jonathan@dockerhost-02:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS      NAMES
deb287c2f0d6   redis     "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   6379/tcp   stoic_panini

jonathan@dockerhost-02:~$ docker exec -it deb287c2f0d6 redis-cli
127.0.0.1:6379> set myvalue 5
OK
127.0.0.1:6379> get myvalue
"5"
127.0.0.1:6379>
```