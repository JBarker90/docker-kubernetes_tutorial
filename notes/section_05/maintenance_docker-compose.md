# Container Maintenance with Compose

### 1. Maintaining Docker Containers that Crash

- If something inside the application causes the Docker container to crash there are several things that we can do to mitigate the issue.

### 2. Showcasing these ways using our `index.js` 

- We added some code on lines 3 and 13 to force all processes to exit when a user visits the root of the project

```
     1  const express = require('express');
     2  const redis = require('redis');
     3  const process = require('process');
     4
     5  const app = express();
     6  const client = redis.createClient({
     7      host: 'redis-server',
     8      port: 6379
     9  });
    10  client.set('visits', 0);
    11
    12  app.get('/', (req, res) => {
    13      process.exit(0);
    14      client.get('visits', (err, visits) => {
    15          res.send('Number of visits is ' + visits);
    16          client.set('visits', parseInt(visits) + 1);
    17      });
    18  });
    19
    20  app.listen(8081, () => {
    21      console.log('Listening on port 8081');
    22  });
```

- Then we can start the container and rebuild the image

```
docker compose up --build
```

Output:

```
jonathan@dockerhost-03:~/docker-kubernetes_tutorial/project-2_visits$ docker compose up --build
[+] Building 1.8s (10/10) FINISHED
 => [internal] load .dockerignore                                                            0.3s  => => transferring context: 2B                                                              0.0s  => [internal] load build definition from Dockerfile                                         0.2s  => => transferring dockerfile: 139B                                                         0.0s  => [internal] load metadata for docker.io/library/node:alpine                               0.6s  => [internal] load build context                                                            0.1s  => => transferring context: 914B                                                            0.0s  => [1/5] FROM docker.io/library/node:alpine@sha256:155e324802ebfdd3f508340dcb0cd4a7510f859  0.0s  => CACHED [2/5] WORKDIR /app                                                                0.0s  => CACHED [3/5] COPY package.json .                                                         0.0s  => CACHED [4/5] RUN npm install                                                             0.0s  => [5/5] COPY . .                                                                           0.3s  => exporting to image                                                                       0.4s  => => exporting layers                                                                      0.4s  => => writing image sha256:5a5feed577e1ebd1b787f07dffa006fab7fc8c01e9028defe1fa2f5b0873046  0.0s  => => naming to docker.io/library/project-2_visits-node-app                                 0.0s [+] Running 3/3
 ⠿ Network project-2_visits_default           Created                                        0.2s  
 ⠿ Container project-2_visits-redis-server-1  Created                                        0.5s  
 ⠿ Container project-2_visits-node-app-1      Cre...                                         0.5s Attaching to project-2_visits-node-app-1, project-2_visits-redis-server-1
project-2_visits-redis-server-1  | 1:C 04 Mar 2023 23:57:03.942 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
project-2_visits-redis-server-1  | 1:C 04 Mar 2023 23:57:03.942 # Redis version=7.0.9, bits=64, commit=00000000, modified=0, pid=1, just started
project-2_visits-redis-server-1  | 1:C 04 Mar 2023 23:57:03.942 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 23:57:03.943 * monotonic clock: POSIX clock_gettime
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 23:57:03.943 * Running mode=standalone, port=6379.
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 23:57:03.943 # Server initialized
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 23:57:03.943 # WARNING Memory overcommit must be enabled! Without it, a background save or replication may fail under low memory condition. Being disabled, it can can also cause failures without low memory condition, see https://github.com/jemalloc/jemalloc/issues/1328. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
project-2_visits-redis-server-1  | 1:M 04 Mar 2023 23:57:03.944 * Ready to accept connections
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | > start
project-2_visits-node-app-1      | > node index.js
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | Listening on port 8081
```

- Now when I go to the URL http://192.168.30.23:4001/ I get an error saying "Connection refused"

```
jonat@Beastmaster /cygdrive/c/Users/jonat
$ curl -I http://192.168.30.23:4001/
curl: (7) Failed to connect to 192.168.30.23 port 4001 after 2044 ms: Connection refused
```

- We also see that the Node.js app is now showing Exited with code 0 from the container output and the Node container has now crashed

```
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | > start
project-2_visits-node-app-1      | > node index.js
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | Listening on port 8081
project-2_visits-node-app-1 exited with code 0

jonathan@dockerhost-03:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS      NAMES
0fbf94cb6a24   redis     "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   6379/tcp   project-2_visits-redis-server-1
```

### 3. How to get Docker compose to restart our Container

- Important Note: In our code, we specified for the node application to exit with a status code of 0. This is essentially saying that the Docker container shutting down was intential. However, the process of how we restart containers will be a little based on the error code

- There are restart policies that you can add to Docker Compose that will specify how Docker should handle restarts
	- "no": Never attempt to restart this . container if it stops or crashes
	- "always": If this container stops *for any reason* always attempt to restart it
	- "on-failure": Only restart if the container stops with an error code
	- "unless-stopped": Always restart unless we (the developers) forcibly stop it

- To test "always" parameter, we can add a line in the Docker compose file for this policy

```
# Version of Docker Compose
version: '3'
services:
  redis-server:
    image: redis
  node-app:
    restart: always
    build: .
    ports:
      - "4001:8081"
```

- After this change, we can bring the containers back up with `docker compose up` and now upon refresh the container restarts itself several times

```
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | > start
project-2_visits-node-app-1      | > node index.js
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | Listening on port 8081
project-2_visits-node-app-1 exited with code 0
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | > start
project-2_visits-node-app-1      | > node index.js
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | Listening on port 8081
project-2_visits-node-app-1 exited with code 0
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | > start
project-2_visits-node-app-1      | > node index.js
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | Listening on port 8081
project-2_visits-node-app-1 exited with code 0
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | > start
project-2_visits-node-app-1      | > node index.js
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | Listening on port 8081
project-2_visits-node-app-1 exited with code 0
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | > start
project-2_visits-node-app-1      | > node index.js
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | Listening on port 8081
project-2_visits-node-app-1 exited with code 0
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | > start
project-2_visits-node-app-1      | > node index.js
project-2_visits-node-app-1      |
project-2_visits-node-app-1      | Listening on port 8081

jonathan@dockerhost-03:~$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS         PORTS                                       NAMES
b89596477824   project-2_visits-node-app   "docker-entrypoint.s…"   2 minutes ago    Up 2 minutes   0.0.0.0:4001->8081/tcp, :::4001->8081/tcp   project-2_visits-node-app-1
0fbf94cb6a24   redis                       "docker-entrypoint.s…"   25 minutes ago   Up 2 minutes   6379/tcp                                    project-2_visits-redis-server-1
```

### 4. Container Status with Docker Compose

- In typical Docker CLI, you will use `docker ps` to list actively running containers. 
- There is a similar Docker compose command that you can use

```
docker-compose ps

docker compose ps
```

Example:

```
jonathan@dockerhost-03:~/docker-kubernetes_tutorial/project-2_visits$ docker compose ps
NAME                              IMAGE                       COMMAND                  SERVICE             CREATED             STATUS              PORTS
project-2_visits-node-app-1       project-2_visits-node-app   "docker-entrypoint.s…"   node-app            44 minutes ago      Up 23 seconds       0.0.0.0:4001->8081/tcp, :::4001->8081/tcp
project-2_visits-redis-server-1   redis                       "docker-entrypoint.s…"   redis-server        44 minutes ago      Up 23 seconds       6379/tcp
```

- When you run `docker compose ps` it will look for a Docker compose file in your current working directory. 