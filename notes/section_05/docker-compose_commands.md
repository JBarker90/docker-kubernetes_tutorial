## Docker Compose Commands

1. Commonly Used Commands:

- Similar to `docker run <myimage>` this is what we can use to start up a docker-compose

```
docker-compose up
```

```
docker compose up
```

- Similar to building an image and running it, we can use one docker-compose command
	- OLD: `docker build .` > `docker run myimage`

```
docker-compose up --build
```

```
docker compose up --build
```

2. Test these commands with our `docker-compose.yml`

- Now in our project directory we can start up the container from the docker-compose file

```
jonathan@dockerhost-03:~/docker-kubernetes_tutorial/project-2_visits$ docker compose up
[+] Running 7/7
 ⠿ redis-server Pulled                                                                      10.7s
   ⠿ 3f9582a2cbe7 Pull complete                                                              2.9s
   ⠿ 241c2d338588 Pull complete                                                              4.8s
   ⠿ 89515d93a23e Pull complete                                                              6.3s
   ⠿ 65e8ba9473fe Pull complete                                                              7.4s
   ⠿ 585124038cab Pull complete                                                              8.5s
   ⠿ b483de716a47 Pull complete                                                              9.0s [+] Building 43.7s (10/10) FINISHED
 => [internal] load build definition from Dockerfile                                         0.6s  => => transferring dockerfile: 139B                                                         0.0s  => [internal] load .dockerignore                                                            0.7s  => => transferring context: 2B                                                              0.0s  => [internal] load metadata for docker.io/library/node:alpine                               2.0s  => [internal] load build context                                                            6.4s  => => transferring context: 1.00kB                                                          0.0s  => [1/5] FROM docker.io/library/node:alpine@sha256:155e324802ebfdd3f508340dcb0cd4a7510f85  26.9s  => => resolve docker.io/library/node:alpine@sha256:155e324802ebfdd3f508340dcb0cd4a7510f859  3.4s  => => sha256:155e324802ebfdd3f508340dcb0cd4a7510f8594802a2e53150f171ae8aa2 1.43kB / 1.43kB  0.0s  => => sha256:51dd437f31812df71108b81385e2945071ec813d5815fa3403855669c8f34 1.16kB / 1.16kB  0.0s  => => sha256:212550fe9cc1c53938c795f335d57c53f30a401cd89fc3e3cae4716e5b785 6.43kB / 6.43kB  0.0s  => => sha256:63b65145d645c1250c391b2d16ebe53b3747c295ca8ba2fcb6b0cf064a4dc 3.37MB / 3.37MB  0.5s  => => sha256:d5bdfeed6dfad70f9df0b629953da51e464fc6b82e13dda206b68775c6f 48.16MB / 48.16MB  3.1s  => => sha256:75ec85813c149650393e1df04572d2d534b75952df3703fd36d33f46c45cb 2.35MB / 2.35MB  2.1s  => => extracting sha256:63b65145d645c1250c391b2d16ebe53b3747c295ca8ba2fcb6b0cf064a4dc21c    0.1s  => => sha256:a6f48326f540df680819ef87dbb9f4011c759628f651a05c85306f796eba1a29 451B / 451B   4.0s  => => extracting sha256:d5bdfeed6dfad70f9df0b629953da51e464fc6b82e13dda206b68775c6f05075    1.3s  => => extracting sha256:75ec85813c149650393e1df04572d2d534b75952df3703fd36d33f46c45cb438    0.1s  => => extracting sha256:a6f48326f540df680819ef87dbb9f4011c759628f651a05c85306f796eba1a29    0.0s  => [2/5] WORKDIR /app                                                                       0.8s  => [3/5] COPY package.json .                                                                0.3s  => [4/5] RUN npm install                                                                    7.3s  => [5/5] COPY . .                                                                           0.5s  => exporting to image                                                                       4.9s  => => exporting layers                                                                      4.4s  => => writing image sha256:85beca750efc28315c7dd69e61e3ca8a140dd1b16638a84060d7bdbaf2abf4b  0.2s  => => naming to docker.io/library/project-2_visits-node-app                                 0.4s [+] Running 3/3
 ⠿ Network project-2_visits_default           Created                                        0.9s  ⠿ Container project-2_visits-redis-server-1  Created                                        0.6s  ⠿ Container project-2_visits-node-app-1      Cre...                                         0.6s Attaching to project-2_visits-node-app-1, project-2_visits-redis-server-1
project-2_visits-redis-server-1  | 1:C 04 Mar 2023 20:48:19.978 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
project-2_visits-redis-server-1  | 1:C 04 Mar 2023 20:48:19.978 # Redis version=7.0.9, bits=64, commit=00000000, modified=0, pid=1, just started
project-2_visits-redis-server-1  | 1:C 04 Mar 2023 20:48:19.978 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 20:48:19.978 * monotonic clock: POSIX clock_gettime
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 20:48:19.979 * Running mode=standalone, port=6379.
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 20:48:19.979 # Server initialized
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 20:48:19.979 # WARNING Memory overcommit must be enabled! Without it, a background save or replication may fail under low memory condition. Being disabled, it can can also cause failures without low memory condition, see https://github.com/jemalloc/jemalloc/issues/1328. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 20:48:19.979 * Ready to accept connections
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | > start
project-2_visits-node-app-1      | > node index.js
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | Listening on port 8081
------
failed to solve: process "/bin/sh -c npm install" did not complete successfully: exit code: 1
```

- Once the containers are up, we can go to `http://192.168.30.23:4001/` and everytime we refresh the page the "Number of visits" goes up



![Project2-browser_test.png](:/499dbab68b2242708119e926f8c4b6d8)

