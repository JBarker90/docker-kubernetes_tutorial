## Listing Running Containers

1. We can use `docker ps`

- This will list all running containers. 

- But currently we do not have any containers that are currently because all the containers we have run so far will exit after completely a task. 

Example: No running containers

```
jonathan@dockerhost-02:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

- However, as a quick proof of concept, we can use `docker run` to run Busybox and ping Google. Then check the container list in a separate terminal window

Terminal 1:

```
jonathan@dockerhost-02:~$ docker run busybox ping google.com
PING google.com (142.250.190.78): 56 data bytes
64 bytes from 142.250.190.78: seq=0 ttl=115 time=15.264 ms
64 bytes from 142.250.190.78: seq=1 ttl=115 time=14.745 ms
64 bytes from 142.250.190.78: seq=2 ttl=115 time=15.847 ms
64 bytes from 142.250.190.78: seq=3 ttl=115 time=14.399 ms
64 bytes from 142.250.190.78: seq=4 ttl=115 time=16.589 ms
```

Terminal 2:

```
jonathan@dockerhost-02:~$ docker ps
CONTAINER ID   IMAGE     COMMAND             CREATED          STATUS          PORTS     NAMES
8bf692298ad9   busybox   "ping google.com"   21 seconds ago   Up 17 seconds             upbeat_bhabha
```

2. If we want to list all containers ever run, we can use the `--all` flag

- This will list all containers ever run regardles of them currently running

```
docker ps --all
```

Example: It will list out all containers ever run. So if there are alot, you might need to `grep` specifically  for an image name

```
jonathan@dockerhost-02:~$ docker ps --all
CONTAINER ID   IMAGE         COMMAND             CREATED         STATUS                     PORTS     NAMES
8bf692298ad9   busybox       "ping google.com"   3 minutes ago   Exited (0) 2 minutes ago             upbeat_bhabha
b1fe0d3587e7   busybox       "ls"                8 days ago      Exited (0) 8 days ago                beautiful_noether

f5f2a36b2ab2   hello-world   "/hello"            8 days ago      Exited (0) 8 days ago                epic_newton
cdf6e7a61f2c   hello-world   "/hello"            12 days ago     Exited (0) 12 days ago               sharp_morse
43293f27b66e   hello-world   "/hello"            12 days ago     Exited (0) 12 days ago               loving_driscoll
710a6460b9ec   hello-world   "/hello"            12 days ago     Created                              vigilant_diffie
5a9d93890f56   hello-world   "/hello"            12 days ago     Exited (0) 12 days ago               quizzical_lederberg
e04484567db6   hello-world   "/hello"            12 days ago     Exited (0) 12 days ago               quizzical_ganguly

f20764c9024f   hello-world   "/hello"            13 days ago     Exited (0) 13 days ago               romantic_shirley de3f73960939   hello-world   "/hello"            13 days ago     Exited (0) 13 days ago               eloquent_clarke
3c058660fcb2   hello-world   "/hello"            13 days ago     Created                              loving_hodgkin
4edca1e50d33   hello-world   "/hello"            13 days ago     Created                              xenodochial_noyce

d2b07dc1be94   hello-world   "/hello"            13 days ago     Created                              nice_galois
e46e07b17c16   hello-world   "/hello"            13 days ago     Created                              nostalgic_pike
```