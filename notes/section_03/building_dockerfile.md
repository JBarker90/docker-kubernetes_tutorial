## Building a Dockerfile

1. First create a docker compose file called `Dockerfile`

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/redis-image$ ll
total 8
drwxrwxr-x 2 jonathan jonathan   3 Jan 24 16:46 ./
drwxrwxr-x 4 jonathan jonathan   5 Jan 24 16:57 ../
-rw-rw-r-- 1 jonathan jonathan 199 Jan 24 17:03 Dockerfile
```

NOTE: The Docker requires that the "D" is capitalized in `Dockerfile`

2. Add some code that specifies a base image and other dependencies

```
# Use an existing docker image as a base 
FROM alpine

# Download and install a dependency
RUN apk add --update redis

# Tell the image what to do when it starts as a container
CMD [ "redis-server" ]
```

3. Make sure you are in the working directory with the `Dockerfile` and build the image.

- This can be done using `docker build`

```
docker build <files to build>
```

Example:

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/redis-image$ docker build .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM alpine
latest: Pulling from library/alpine
8921db27df28: Pull complete
Digest: sha256:f271e74b17ced29b915d351685fd4644785c6d1559dd1f2d4189a5e851ef753a
Status: Downloaded newer image for alpine:latest
 ---> 042a816809aa
Step 2/3 : RUN apk add --update redis
 ---> Running in 0c3182e61e55
fetch https://dl-cdn.alpinelinux.org/alpine/v3.17/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.17/community/x86_64/APKINDEX.tar.gz
(1/1) Installing redis (7.0.8-r0)
Executing redis-7.0.8-r0.pre-install
Executing redis-7.0.8-r0.post-install
Executing busybox-1.35.0-r29.trigger
OK: 10 MiB in 16 packages
Removing intermediate container 0c3182e61e55
 ---> 6ec23aeb3d37
Step 3/3 : CMD [ "redis-server" ]
 ---> Running in 6a43850b751a
Removing intermediate container 6a43850b751a
 ---> b7dcf29f7cb6
Successfully built b7dcf29f7cb6
```

4. Then run the build.

- This can be done with `docker run`

```
docker run <image id>
```

Example: This will run Redis and wait for some update from the user

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/redis-image$ docker run b7dcf29f7cb6
1:C 24 Jan 2023 17:12:18.653 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 24 Jan 2023 17:12:18.653 # Redis version=7.0.8, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 24 Jan 2023 17:12:18.653 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 24 Jan 2023 17:12:18.654 * monotonic clock: POSIX clock_gettime
1:M 24 Jan 2023 17:12:18.654 * Running mode=standalone, port=6379.
1:M 24 Jan 2023 17:12:18.654 # Server initialized
1:M 24 Jan 2023 17:12:18.654 # WARNING Memory overcommit must be enabled! Without it, a background save or replication may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 24 Jan 2023 17:12:18.654 * Ready to accept connections
```
