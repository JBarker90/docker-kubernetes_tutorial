# Introducing Docker Compose

### 1. What does Docker Compose Accomplish

- When modifying Docker container networking, using Docker compose is the easiest solution.

- Using Docker compose, we can connect our Node app container to a separate Redis container without relying on Docker CLI's Network features only. 

- We have two options when connecting containers:
	- Use Docker CLI's Network Features (more of a pain to use)
	- Use Docker Compose

### 2. What is Docker Compose

- Separate CLI that gets installed along with Docker
- Used to start up multiple Docker containers at the same time
- Automates some of the long-winded arguments we were passing to 'docker run'

### 3. Docker Compose Files

- We will be taking various commands like `docker build` or `docker run` and incorporate them into a `docker-compose.yml`
- Each command will be structured in the file in a YAML format. (NOT copy/pasted)
- Then, we will take the `docker-compose.yml` file and send it to docker-compose CLI
- Overall Breakdown of what the `docker-compose.yml` file will look like
	- Here are the containers I want created:
		- redis-server:
			- Make it using the 'redis' image
		- node-app:
			- Make it using the Dockerfile in the current directory
			- Map port 8081 to 8081

- Now we can create a new `docker-compose.yml` file

```
# Version of Docker Compose
version: '3'
services:
  redis-server:
    image: redis
  node-app:
    build: .
    ports:
      - "4001:8081"
```