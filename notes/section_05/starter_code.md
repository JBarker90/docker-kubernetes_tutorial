## App Server Starter Code

1. Starter Code

- Created a new project directory called `project-2_visits`

```
/home/jonathan/docker-kubernetes_tutorial/project-2_visits
```

- Created a `package.json` file

```
{
    "dependencies": {
        "express": "*",
        "redis": "2.8.0"
    },
    "scripts": {
        "start": "node index.js"
    }
}
```

- Created an `index.js` file

```
const express = require('express');
const redis = require('redis');

const app = express();
const client = redis.createClient();
client.set('visits', 0);

app.get('/', (req, res) => {
    client.get('visits', (err, visits) => {
        res.send('Number of visits is ' + visits);
        client.set('visits', parseInt(visits) + 1);
    });
});

app.listen(8081, () => {
    console.log('Listening on port 8081');
});
```