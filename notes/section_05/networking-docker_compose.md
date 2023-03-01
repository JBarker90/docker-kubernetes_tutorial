## Networking with Docker Compose

1. What does Docker Compose solve

- Earlier, when we manually created containers Docker would create each container isolated in it's own network.
	
	- So there was no way, without manual configuration, for the containers to easily communicate with each other.

- When creating a Docker Compose file, Docker will automatically connect any containers that you specify into the same network. 

2. Connecting our Project to Redis

- In the `index.js` file there is a redis client configuration where we can specify the host as 'redis-server'

```
const express = require('express');
const redis = require('redis');

const app = express();
const client = redis.createClient({
    host: 'redis-server'
});
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