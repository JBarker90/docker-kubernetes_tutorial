# Stopping Containers

### 1. How to Stop a currently Running Container

- There are two options that we have `docker stop` or `docker kill`

Docker Stop: 

- This will send a SIGTERM signal to the running process and tells the container to shutdown running processes and other programs cleanly on its own.

```
docker stop <container id>
```

NOTE: The stop command will usually run for about 10 seconds and then automatically issue the kill command. 

Docker Kill:

- This will send a SIGKILL signal to the running process and tells the container to shutdown now without additional work. It's essentially pulling the plug from the running process. 

```
docker kill <container id>
```
