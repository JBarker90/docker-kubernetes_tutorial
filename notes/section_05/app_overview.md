## Project 2 - App Overview

1. App Overview

- The second project will be a simple web application that counts the number of visits to the page.

2. Our goal with Docker

- We need to create the Node application in one Docker container and utilize Redis in a second Docker container for scalability.



![Project 2-app_env.png](:/9b3dcf49073242d1a01077b2439ada83)

- The goal is to keep the application in a seperate container from Redis because we can always scale to add more running containers for the app. 

-  If they are all connected to the same Redis instance then there will not be a discrepancy in the number of visits



![Project 2-app_env(scaled).png](:/b59596be4f9845e68e99d7ff697c5d83)

