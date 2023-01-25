# Dealing With Stopped Containers

### 1. Restarting Stopped Containers:

- Sometimes a container will be exited and shutdown for various reasons. 

- If you want to restart a container, you can use `docker start` against the image ID

```
docker start -a <ImageID>
```

Example: I ran a container > Grabbed Image ID from list > restarted that specific container

```
jonathan@dockerhost-02:~$ docker run busybox echo hi there
hi there

jonathan@dockerhost-02:~$ docker ps --all | head -n3
CONTAINER ID   IMAGE         COMMAND             CREATED          STATUS                      PORTS     NAMES
f89a07a8d5a8   busybox       "echo hi there"     13 seconds ago   Exited (0) 12 seconds ago             zealous_fermat 8bf692298ad9   busybox       "ping google.com"   20 minutes ago   Exited (0) 18 minutes ago             upbeat_bhabha

jonathan@dockerhost-02:~$ docker start -a f89a07a8d5a8
hi there
```

NOTE: The `-a` in `docker start -a <ImageID>` will tell the docker client to print the output of the comand to the terminal

### 2. Removing Stopped Containers

- You can use the `system prune` option in Docker CLI

```
docker system prune
```

- This will prompt you with warning message. But just be sure you want to remove all stopped containers along with build cache

Example:

```
jonathan@dockerhost-02:~$ docker ps --all
CONTAINER ID   IMAGE         COMMAND             CREATED          STATUS                      PORTS     NAMES
f89a07a8d5a8   busybox       "echo hi there"     9 minutes ago    Exited (0) 7 minutes ago              zealous_fermat 8bf692298ad9   busybox       "ping google.com"   29 minutes ago   Exited (0) 27 minutes ago             upbeat_bhabha
b1fe0d3587e7   busybox       "ls"                8 days ago       Exited (0) 8 days ago                 beautiful_noether
f5f2a36b2ab2   hello-world   "/hello"            8 days ago       Exited (0) 8 days ago                 epic_newton
cdf6e7a61f2c   hello-world   "/hello"            13 days ago      Exited (0) 13 days ago                sharp_morse
43293f27b66e   hello-world   "/hello"            13 days ago      Exited (0) 13 days ago                loving_driscoll710a6460b9ec   hello-world   "/hello"            13 days ago      Created                               vigilant_diffie5a9d93890f56   hello-world   "/hello"            13 days ago      Exited (0) 13 days ago                quizzical_lederberg
e04484567db6   hello-world   "/hello"            13 days ago      Exited (0) 13 days ago                quizzical_ganguly
f20764c9024f   hello-world   "/hello"            13 days ago      Exited (0) 13 days ago                romantic_shirley
de3f73960939   hello-world   "/hello"            13 days ago      Exited (0) 13 days ago                eloquent_clarke3c058660fcb2   hello-world   "/hello"            13 days ago      Created                               loving_hodgkin 4edca1e50d33   hello-world   "/hello"            13 days ago      Created                               xenodochial_noyce
d2b07dc1be94   hello-world   "/hello"            13 days ago      Created                               nice_galois
e46e07b17c16   hello-world   "/hello"            13 days ago      Created                               nostalgic_pike

jonathan@dockerhost-02:~$ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y
Deleted Containers:
f89a07a8d5a8628f217aea6e5239ec562022f245db3f49213c8a65a480b653cd
8bf692298ad91d3a466ee3430a473f3e20db29a1efd381394fa1c065fa7332c2
b1fe0d3587e798a68c0252e18f4d202f1e54d95656229f4c276849de7cdb7740
f5f2a36b2ab2ae15a9191eba7e8609a0f9dadcb932748cd4062138a41701956b
cdf6e7a61f2ca9364e2fb86afdeec68afb126ad76decfd8f978d7deb363c9ce9
43293f27b66ef65deafafaf1034f011c7167978e030ad8565398f04babe4195a
710a6460b9ece8d85ce258bcbf059f359cbdc263f06122fa07688a7cfe893ad4
5a9d93890f56af28911ba8c41600391a546b67b0866997834472b4684e74c809
e04484567db6b0b898ac0edcfcd8d948b9e41b33a024606dd8b9c184b5bef9a9
f20764c9024f22ebe463d5a518eb2ff8c7069f1964ec3569125aadf26ae89402
de3f739609395238ee3ebb9c2d5ed9ddc5c6252648300c8b3df7de4d5a8a4db1
3c058660fcb25b072892368bc67dcc88d47297ca4a9453891cfceb079d2ba92f
4edca1e50d338ef022041624964d94a20684c8272b07a2ed9ac5313014e037f6
d2b07dc1be94fa18c62423e6ca8ebd5f7c18cb7765a58f8a99ba158c3e365c00
e46e07b17c16e4b35a20b5a4e6f0acd9d4e7bc3da7620df08eae09847dae7698

Total reclaimed space: 0B

jonathan@dockerhost-02:~$ docker ps --all
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

NOTE: This can be very useful when cleaning up disk space. 