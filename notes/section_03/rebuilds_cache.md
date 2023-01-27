## Rebuilds With Cache

1. Docker Build using Cache

- The main performance gains from rebuilting images by using cache.
- To show this, we can add a new RUN line in our dockerfile that does the following 

```
RUN apk add --update gcc
```

- This will simply add a new dependency to our build to showcase the use of cache

```
# Use an existing docker image as a base 
FROM alpine

# Download and install a dependency
RUN apk add --update redis
RUN apk add --update gcc

# Tell the image what to do when it starts as a container
CMD [ "redis-server" ]
```

2. Seeing Cache during build process

- Now, after adding the new dependency we can rerun `docker build` 

```
docker build .
```

- From the output, you can see that Docker is utilizing cache assets for the Redis image and does not need to redownload anything

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/redis-image$ docker build .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM alpine
 ---> 042a816809aa
Step 2/4 : RUN apk add --update redis
 ---> Using cache
 ---> 6ec23aeb3d37
Step 3/4 : RUN apk add --update gcc
 ---> Running in 86e3c40f039b
fetch https://dl-cdn.alpinelinux.org/alpine/v3.17/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.17/community/x86_64/APKINDEX.tar.gz
(1/10) Installing libgcc (12.2.1_git20220924-r4)
(2/10) Installing libstdc++ (12.2.1_git20220924-r4)
(3/10) Installing binutils (2.39-r2)
(4/10) Installing libgomp (12.2.1_git20220924-r4)
(5/10) Installing libatomic (12.2.1_git20220924-r4)
(6/10) Installing gmp (6.2.1-r2)
(7/10) Installing isl25 (0.25-r0)
(8/10) Installing mpfr4 (4.1.0-r0)
(9/10) Installing mpc1 (1.2.1-r1)
(10/10) Installing gcc (12.2.1_git20220924-r4)
Executing busybox-1.35.0-r29.trigger
OK: 145 MiB in 26 packages
Removing intermediate container 86e3c40f039b
 ---> 50004438b934
Step 4/4 : CMD [ "redis-server" ]
 ---> Running in e76d22654012
Removing intermediate container e76d22654012
 ---> e6a706c13e6a
Successfully built e6a706c13e6a
```

3. Docker will not be able to reuse cache if the Order in build changes

- To showcase this, we can switch around the order of the GCC and Redis dependencies. Doing this the cache will be invalidated and forced to rebuild it's cache.

```
# Use an existing docker image as a base 
FROM alpine

# Download and install a dependency
RUN apk add --update gcc
RUN apk add --update redis

# Tell the image what to do when it starts as a container
CMD [ "redis-server" ]
```

- Since the order of operations has changed, Docker cannot use cache and rebuilds the image 

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/redis-image$ docker build .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM alpine
 ---> 042a816809aa
Step 2/4 : RUN apk add --update gcc
 ---> Running in ad61c677260d
fetch https://dl-cdn.alpinelinux.org/alpine/v3.17/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.17/community/x86_64/APKINDEX.tar.gz
(1/10) Installing libgcc (12.2.1_git20220924-r4)
(2/10) Installing libstdc++ (12.2.1_git20220924-r4)
(3/10) Installing binutils (2.39-r2)
(4/10) Installing libgomp (12.2.1_git20220924-r4)
(5/10) Installing libatomic (12.2.1_git20220924-r4)
(6/10) Installing gmp (6.2.1-r2)
(7/10) Installing isl25 (0.25-r0)
(8/10) Installing mpfr4 (4.1.0-r0)
(9/10) Installing mpc1 (1.2.1-r1)
(10/10) Installing gcc (12.2.1_git20220924-r4)
Executing busybox-1.35.0-r29.trigger
OK: 142 MiB in 25 packages
Removing intermediate container ad61c677260d
 ---> 0edb346ae21f
Step 3/4 : RUN apk add --update redis
 ---> Running in 41df373048c0
(1/1) Installing redis (7.0.8-r0)
Executing redis-7.0.8-r0.pre-install
Executing redis-7.0.8-r0.post-install
Executing busybox-1.35.0-r29.trigger
OK: 145 MiB in 26 packages
Removing intermediate container 41df373048c0
 ---> 86a0cb5b8faa
Step 4/4 : CMD [ "redis-server" ]
 ---> Running in 09202f7aadf5
Removing intermediate container 09202f7aadf5
 ---> 7cc767f07a59
Successfully built 7cc767f07a59
```