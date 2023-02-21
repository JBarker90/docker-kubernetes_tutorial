# Container Port Mapping

### 1. Build from Previous project is listening on port `8080`. But we cannot connect through Browser

- To resolve this issue, we need to explicitly map the local port of 8080 to the container port 8080

- Docker containers will have their own isolated networking


![Container_networking.png](https://github.com/JBarker90/docker-kubernetes_tutorial/blob/main/notes/section_04/Container_networking.png)

NOTE: This is only talking about Incoming network requests. 

### 2. Mapping Ports

- Port mapping is only done when starting up the container using `docker run`

- This will be the syntax you can use

```
docker run -p <src port>:<dest port> <image id>
```

Example:

```
docker run -p 8080:8080 <image id>
```

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/simpleweb$ docker run -p 8080:8080 jbarker09/simpleweb

> @ start /
> node index.js

Listening on port 8080

jonathan@dockerhost-02:~$ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                       NAMES
508ee1c605fe   jbarker09/simpleweb   "docker-entrypoint.s…"   23 seconds ago   Up 20 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   great_khorana
```

- Now, the page loads correctly in the browser. 



![Browser-loads.png](https://github.com/JBarker90/docker-kubernetes_tutorial/blob/main/notes/section_04/Browser-loads.png)

- The ports on the source and destination do NOT need to match. So you can map which ever local port you need

```
docker run -p 5000:8080 jbarker09/simpleweb
```

```
jonathan@dockerhost-02:~$ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                       NAMES
eb03ef656c96   jbarker09/simpleweb   "docker-entrypoint.s…"   38 seconds ago   Up 35 seconds   0.0.0.0:5000->8080/tcp, :::5000->8080/tcp   angry_hellman
```



![Browser-loads_port5000.png](https://github.com/JBarker90/docker-kubernetes_tutorial/blob/main/notes/section_04/Browser-loads_port5000.png)

