## Specifying a Working Directory

1. Copying Files to a Nested Directory

- Earlier when we copied the project files to the container using the Dockerfile, our files were copied to the root directory of the container. 

- You can run a terminal with a shell using the below command

```
docker run -it jbarker09/simpleweb sh
```

```
/ # ls -l | grep 'package*'
-rw-r--r--    1 root     root         20404 Feb 21 16:47 package-lock.json
-rw-rw-r--    1 root     root           112 Feb 21 16:47 package.json

/ # ls -l | grep index*
-rw-rw-r--    1 root     root           191 Feb 14 17:25 index.js

/ # pwd
/
```

NOTE: This can cause issues because there might be application files/directories that are named the same thing as the containers root file system (var, lib, or etc). This can cause issues with the container if important files/directories conflict and become overwritten.

- To resolve this issue, we can specify our working directory in the Dockerfile using syntax like this

```
WORKDIR /usr/app
```

- In `Dockerfile`

```
# Specify a base image
FROM node:14-alpine

WORKDIR /usr/app

# Install some dependencies
COPY ./ ./
RUN npm install

# Default command
CMD [ "npm", "start" ]
```

2. After modifying the working directory, we need to rebuild it

- We can run `docker build` again and tag the image name

```
docker build -t jbarker09/simpleweb .
```

Output:

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/simpleweb$ docker build -t jbarker09/simpleweb .
Sending build context to Docker daemon  4.096kB
Step 1/5 : FROM node:14-alpine
 ---> 4802e63f08a5
Step 2/5 : WORKDIR /usr/app
 ---> Running in 869bdb9447d7
Removing intermediate container 869bdb9447d7
 ---> 6cd14387030f
Step 3/5 : COPY ./ ./
 ---> 09c35c8c1da4
Step 4/5 : RUN npm install
 ---> Running in b2a048af6716
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN app No description
npm WARN app No repository field.
npm WARN app No license field.

added 57 packages from 42 contributors and audited 57 packages in 2.672s

7 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

Removing intermediate container b2a048af6716
 ---> a3b080f1feac
Step 5/5 : CMD [ "npm", "start" ]
 ---> Running in 8d9b40fcdecf
Removing intermediate container 8d9b40fcdecf
 ---> 234d69ab105d
Successfully built 234d69ab105d
Successfully tagged jbarker09/simpleweb:latest
```

- After rebuilding the Dockerfile, we can start up the container and attach to a shell to verify everything works properly

```
docker run -p 8080:8080 jbarker09/simpleweb

docker exec -it c9aa73868f22 sh
```

NOTE: You can grab the container ID using `docker ps`

Output:

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/simpleweb$ docker exec -it c9aa73868f22 sh
/usr/app # pwd
/usr/app

/usr/app # ls -l
total 38
-rw-rw-r--    1 root     root           160 Feb 22 17:01 Dockerfile
-rw-rw-r--    1 root     root           191 Feb 14 17:25 index.js
drwxr-xr-x   59 root     root            59 Feb 22 17:06 node_modules
-rw-r--r--    1 root     root         20404 Feb 22 17:06 package-lock.json
-rw-rw-r--    1 root     root           112 Feb 22 17:06 package.json
```