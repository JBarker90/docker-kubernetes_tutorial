# Development Workflow Specifics

### 1. What is Development Flow

- This section will cover the general Development Flow of Developing an application > Testing > Deploying to remote environment like AWS

### 2. Flow Specifics

- Projects will first be created on a Git repository somewhere like GitHub. 
	- Then, you (on local machine) clone the repo from GitHub to your machine and push new changes to a "Feature branch". (Never push changes to the Master branch)
	- From the "Feature branch", you submit a pull request to merge changes to the Master branch
	- From there, the Master Branch is cycled through a CI/CD pipeline like Travis CI
	- Then, Travis CI deploys everything to the production AWS server
	- Then, cycles back to Stage 1 (development)


![Dev-workflow.png](:/a7532cf26ae14a7aab37d388bb7545b2)

### 3. Docker's Purpose

- Docker is NOT necessary to this development workflow. It just makes things easier when you are piping a build of your project through the CI/CD pipeline

- Docker is a tool in a normal development workflow.
