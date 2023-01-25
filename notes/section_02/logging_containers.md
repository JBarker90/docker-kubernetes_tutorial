# Retrieving Log Output

### 1. Retrieving Log Output

- If it is costly to run/re-run a Docker container, you can use `docker logs` to get an output of the container 

```
docker logs <ContainerID>
```

Example:

```
jonathan@dockerhost-02:~$ docker ps --all
CONTAINER ID   IMAGE     COMMAND           CREATED          STATUS                          PORTS     NAMES
2ca03f190bf9   busybox   "echo hi there"   15 minutes ago   Exited (0) About a minute ago             mystifying_torvalds

jonathan@dockerhost-02:~$ docker logs 2ca03f190bf9
hi there
```