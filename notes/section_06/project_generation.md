# Project Generation

### 1. What Will the Project Consist

- This will not have custom code. But it will be a simple React application that we will wrap up in a Docker container

- After wrapping it in a Docker container, we will learn how to deploy it to AWS automatically.  

- In order to move forward with this project, we need to make sure Nodejs and NPM is installed 

```
jonathan@dockerhost-03:~$ node -v
v12.22.9
```

NOTE: If it's not found, you'll need to install it. 

### 2. Generating React Application

- From the terminal, you can run the following command to generate an application called "frontend"

```
npx create-react-app frontend
```

Example:

```
jonathan@dockerhost-03:~/docker-kubernetes_tutorial/project-3_react$ npx create-react-app frontend

Creating a new React app in /home/jonathan/docker-kubernetes_tutorial/project-3_react/frontend.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts with cra-template...


added 1417 packages in 32s

231 packages are looking for funding
  run `npm fund` for details

Installing template dependencies using npm...

added 62 packages in 5s

Success! Created frontend at /home/jonathan/docker-kubernetes_tutorial/project-3_react/frontend
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd frontend
  npm start

Happy hacking!
```

### 3. Necessary Commands

- Here are some commands that we should be familiar with during this project
	- `npm run start` : Starts up a development server. *For development use only*
	- `npm run test` : Runs tests associated with the project
	- `npm run build` : Builds a **production** version of the application