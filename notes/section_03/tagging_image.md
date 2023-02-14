## Tagging an Image

1. Tagging an image by Name

- When we run an image from a Dockerfile build, we typically run using the container ID

```
docker run 7cc767f07a59
```

NOTE: This is the container ID from earlier Docker build.

- However, it is much nicer to be able to run an image using something like

```
docker run redis
```

- This is what we will run to create the build. 

Syntax:

```
docker build -t <dockerid>/<projectname>:<version> .
```

```
docker build -t jbarker09/redis:latest .
```

NOTE: The Docker ID will be your username for Docker Hub. 

Example:

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/redis-image$ ll
total 19
drwxrwxr-x 2 jonathan jonathan   3 Jan 24 16:46 ./
drwxrwxr-x 5 jonathan jonathan   6 Jan 24 17:52 ../
-rw-rw-r-- 1 jonathan jonathan 224 Jan 27 17:13 Dockerfile

jonathan@dockerhost-02:~/docker-kubernetes_tutorial/redis-image$ docker build -t jbarker09/redis:latest .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM alpine
 ---> 042a816809aa
Step 2/4 : RUN apk add --update gcc
 ---> Using cache
 ---> 0edb346ae21f
Step 3/4 : RUN apk add --update redis
 ---> Using cache
 ---> 86a0cb5b8faa
Step 4/4 : CMD [ "redis-server" ]
 ---> Using cache
 ---> 7cc767f07a59
Successfully built 7cc767f07a59
Successfully tagged jbarker09/redis:latest
```

2. How to run the new Build

- You can run it using `docker run`

```
docker run <dockerid>/<projectname>
```

Example:

```
docker run jbarker09/redis
```
