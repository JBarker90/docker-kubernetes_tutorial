## Assembling a Dockerfile

1. This will be very similar to what we did Before

- This Dockerfile will only be for the Node app container

```
FROM node:alpine

WORKDIR '/app'

COPY package.json .
RUN npm install
COPY . .

CMD [ "npm", "start" ]
```

2. Then we can build it from the `Dockerfile`

- We can build it with a tag

```
docker build -t jbarker09/project-2_visits:lastest .
```

Output:

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/project-2_visits$ docker build -t jbarker09/project-2_visits:lastest .
Sending build context to Docker daemon  4.096kB
Step 1/6 : FROM node:alpine
 ---> 212550fe9cc1
Step 2/6 : WORKDIR '/app'
 ---> Using cache
 ---> bd144e35f354
Step 3/6 : COPY package.json .
 ---> Using cache
 ---> aac17d6f2def
Step 4/6 : RUN npm install
 ---> Using cache
 ---> 3813d9b03f10
Step 5/6 : COPY . .
 ---> Using cache
 ---> afc191099495
Step 6/6 : CMD [ "npm", "start" ]
 ---> Using cache
 ---> f219fda56999
Successfully built f219fda56999
Successfully tagged jbarker09/project-2_visits:lastest
```