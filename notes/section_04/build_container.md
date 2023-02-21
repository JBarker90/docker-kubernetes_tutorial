# How We Are Going to Build the Container

### 1. Write Code Base:

- For the project, we wrote some simple Node.js

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/simpleweb$ ll
total 20
drwxrwxr-x 2 jonathan jonathan   4 Feb 14 17:20 ./
drwxrwxr-x 6 jonathan jonathan   7 Feb 14 17:14 ../
-rw-rw-r-- 1 jonathan jonathan 191 Feb 14 17:25 index.js
-rw-rw-r-- 1 jonathan jonathan 111 Feb 14 17:18 package.json
```

- Code in `index.js`

```
     1  const express = require('express');
     2
     3  const app = express();
     4
     5  app.get('/', (req, res) => {
     6      res.send('Hi there')
     7  });
     8
     9  app.listen(8080, () => {
    10      console.log('Listening on port 8080')
    11  });
```

- Code in `package.json`

```
     1  {
     2      "dependencies": {
     3          "express": "*"
     4      },
     5      "scripts": {
     6          "start": "node index.js"
     7      }
     8  }
```

### 2. Flow of Docker File

![Flow-Dockerfile.png](:/13992dd7a654426794c0323b30c2d048)

- Template:
	- Specify a base image
	- Run some commands to install additional programs
	- Specify a command to run on container startup

- Redis:
	- FROM alpine
	- RUN apk add --update redis
	- CMD ["redis-server"]

- Node:
	- FROM alpine
	- RUN npm install
	- CMD ["npm", "start"]

- Create `Dockerfile`

```
# Specify a base image
FROM alpine

# Install some dependencies
RUN npm install

# Default command
CMD [ "npm", "start" ]
```

NOTE: We are doing some things incorrect on purpose.

- Build Dockerfile

```
docker build .
```

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/simpleweb$ docker build .
Sending build context to Docker daemon  4.096kB
Step 1/3 : FROM alpine
 ---> 042a816809aa
Step 2/3 : RUN npm install
 ---> Running in 281234eb9ea8
/bin/sh: npm: not found
The command '/bin/sh -c npm install' returned a non-zero code: 127
```

### 3. Docker Build Failed

- From the output the build failed because `npm` was not found.

- The issue was caused by our base image being Alpine, which does NOT come with `npm` by default.

### 4. How to Resolve this Base Image Issue:

- Two options:
	- We can use a different base image that already has Node and `npm` installed
	- We can continue to use Alpine and install additional dependency with Node and `npm` inside our Alpine image.

- In our case, we will go to https://hub.docker.com/ and use a base image with Node and `npm` already installed.
	- From Docker Hub, we can use the Node base image and use the Alpine tag 
	- https://hub.docker.com/_/node

```
# Specify a base image
FROM node:14-alpine

# Install some dependencies
RUN npm install

# Default command
CMD [ "npm", "start" ]
```

### 5. Now that we resolved the Node issue, we can try building:

- After changing base image, we can run `docker build` again

```
docker build .
```

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/simpleweb$ docker build .
Sending build context to Docker daemon  4.096kB
Step 1/3 : FROM node:14-alpine
14-alpine: Pulling from library/node
63b65145d645: Pull complete
9152c2de31c0: Pull complete
72d96f5d8c21: Pull complete
2fb18465637a: Pull complete
Digest: sha256:86c59eb57b10df3d55e460b28799f60121f950ad018ff0989ea01ab61a1d9ab2
Status: Downloaded newer image for node:14-alpine
 ---> 4802e63f08a5
Step 2/3 : RUN npm install
 ---> Running in cde1ad121191
npm WARN saveError ENOENT: no such file or directory, open '/package.json'
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN enoent ENOENT: no such file or directory, open '/package.json'
npm WARN !invalid#2 No description
npm WARN !invalid#2 No repository field.
npm WARN !invalid#2 No README data
npm WARN !invalid#2 No license field.

up to date in 0.321s
found 0 vulnerabilities

Removing intermediate container cde1ad121191
 ---> 3c487a0d6b77
Step 3/3 : CMD [ "npm", "start" ]
 ---> Running in e85a34489661
Removing intermediate container e85a34489661
 ---> 87d39f0af9b7
Successfully built 87d39f0af9b7
```

NOTE: Now we are seeing a different error message about `package.json` missing. This is because you need to allow specific files (like package.json and index.js) to be included in the Dockerfile.

```
npm WARN saveError ENOENT: no such file or directory, open '/package.json'
```

### 6. Copying Build Files:

- In the Dockerfile, we can include a `COPY` line that will copy files from your machine to the container build process.

Syntax:

```
COPY <PathFromYourMachine> <PathInsideContainer>
```

```
# Specify a base image
FROM node:14-alpine

# Install some dependencies
COPY ./ ./
RUN npm install

# Default command
CMD [ "npm", "start" ]
```

- Now when we run `docker build .` we resolved the `package.json` issue and created the correct build. 

- However, when we run this new build it generates a default Container ID. So we can tag the image to make it human readable

```
docker build -t jbarker09/simpleweb .
```

Output:

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/simpleweb$ docker build -t jbarker09/simpleweb .
Sending build context to Docker daemon  4.096kB
Step 1/4 : FROM node:14-alpine
 ---> 4802e63f08a5
Step 2/4 : COPY ./ ./
 ---> Using cache
 ---> e0ac14829dd7
Step 3/4 : RUN npm install
 ---> Using cache
 ---> c2592a7286cf
Step 4/4 : CMD [ "npm", "start" ]
 ---> Using cache
 ---> 940c0bc22fa9
Successfully built 940c0bc22fa9
Successfully tagged jbarker09/simpleweb:latest
```

### 7. Then Run a Container From Image

- Now that we have a new build, we can run a new container from the image

```
docker run jbarker09/simpleweb
```

Output:

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/simpleweb$ docker run jbarker09/simpleweb

> @ start /
> node index.js

Listening on port 8080

jonathan@dockerhost-02:~$ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS         PORTS     NAMES
50815c85403c   jbarker09/simpleweb   "docker-entrypoint.s…"   10 seconds ago   Up 8 seconds             gracious_mclaren
```

