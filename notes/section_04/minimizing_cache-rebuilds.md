# Minimizing Cache Busting and Rebuilds

### 1. Avoiding Reinstallation of Dependencies

- When we changed out working directory, Docker had to reinstall all dependencies. This can be very costly and time consuming in large builds

- We can take the `COPY` process in the Dockerfile and split it into 2 different steps. The only thing we need to copy over is `package.json`. This is the only file the `npm` installer cares about.

```
COPY ./package.json ./
```

- Then, after the `npm install` process add another `COPY` process to copy everything over

```
COPY ./ ./
```

Example:

```
# Specify a base image
FROM node:14-alpine

WORKDIR /usr/app

# Install some dependencies
COPY ./package.json ./
RUN npm install
COPY ./ ./

# Default command
CMD [ "npm", "start" ]
```

- Then, rebuild it

```
docker build -t jbarker09/simpleweb .
```

### 2. Now, let's assume you want to make a change to the `index.js` file

- We can simply change the message to be "How are you doing"

```
const express = require('express');

const app = express();

app.get('/', (req, res) => {
    res.send('How are you doing')
});

app.listen(8080, () => {
    console.log('Listening on port 8080')
});
```

- Now when we rebuild the image Docker doesn't have to reinstall all dependencies

```
jonathan@dockerhost-02:~/docker-kubernetes_tutorial/simpleweb$ docker build -t jbarker09/simpleweb .
Sending build context to Docker daemon  4.096kB
Step 1/6 : FROM node:14-alpine
 ---> 4802e63f08a5
Step 2/6 : WORKDIR /usr/app
 ---> Using cache
 ---> 6cd14387030f
Step 3/6 : COPY ./package.json ./
 ---> Using cache
 ---> 8320fbdc91ef
Step 4/6 : RUN npm install
 ---> Using cache
 ---> 842129d1d0e1
Step 5/6 : COPY ./ ./
 ---> 1ac19fb06ce8
Step 6/6 : CMD [ "npm", "start" ]
 ---> Running in afdb2b428442
Removing intermediate container afdb2b428442
 ---> 760fb701a686
Successfully built 760fb701a686
Successfully tagged jbarker09/simpleweb:latest
```

NOTE: The build process will only copy over the `index.js` file that changed.