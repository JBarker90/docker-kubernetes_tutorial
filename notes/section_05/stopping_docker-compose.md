## Stopping Docker Compose Containers

1. Containers Running in the Background Starting/Stopping Manually

- We can run a container manually in the background using the `-d` flag for detached 

```
docker run -d <image_name>
```

Example:

```
jonathan@dockerhost-03:~/docker-kubernetes_tutorial/project-2_visits$ docker run -d redis
6567c6cb8a6460b3d4ad17963c370030bdb66b2f2046f2f73e3cb1d733391fa5

jonathan@dockerhost-03:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS      NAMES
6567c6cb8a64   redis     "docker-entrypoint.s…"   46 seconds ago   Up 44 seconds   6379/tcp   nifty_curran
```

- Then, we can manually stop the container using `docker stop`

```
docker stop <container_id>
```

```
jonathan@dockerhost-03:~/docker-kubernetes_tutorial/project-2_visits$ docker stop 6567c6cb8a64
6567c6cb8a64

jonathan@dockerhost-03:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

2. Starting/Stopping Containers using Docker Compose

- To launch a container in the background using Docker Compose we can use this command

```
docker-compose up -d

docker compose up -d
```

Example:

```
jonathan@dockerhost-03:~/docker-kubernetes_tutorial/project-2_visits$ docker compose up -d
[+] Running 2/2
 ⠿ Container project-2_visits-node-app-1      Sta...                                         0.8s  
 ⠿ Container project-2_visits-redis-server-1  Started                                        0.9s

jonathan@dockerhost-03:~$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED       STATUS
    PORTS                                       NAMES
24006a9964a0   project-2_visits-node-app   "docker-entrypoint.s…"   3 hours ago   Up About a minute   0.0.0.0:4001->8081/tcp, :::4001->8081/tcp   project-2_visits-node-app-1
d4f91c59d063   redis                       "docker-entrypoint.s…"   3 hours ago   Up About a minute   6379/tcp                                    project-2_visits-redis-server-1
```

- To stop a container running in the background using Docker compose

```
docker-compose down

docker compose down
```

Example:

```
jonathan@dockerhost-03:~/docker-kubernetes_tutorial/project-2_visits$ docker compose down
[+] Running 3/3
 ⠿ Container project-2_visits-node-app-1      Rem...                                         2.5s  
 ⠿ Container project-2_visits-redis-server-1  Removed                                        2.4s  
 ⠿ Network project-2_visits_default           Removed                                        0.3s
 
jonathan@dockerhost-03:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES 
```