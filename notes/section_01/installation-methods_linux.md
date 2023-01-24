# Installation Methods for Docker on Linux

These steps listed below are for Ubuntu Desktop LTS. You can find the full official docs and steps for other Linux distributions here:

- Ubuntu: https://docs.docker.com/install/linux/docker-ce/ubuntu/

- CentOS: https://docs.docker.com/install/linux/docker-ce/centos/
(Note: Students have encountered issues when using CentOS or RHEL as the host related to Docker container communication. You may need to research some workaround for the errors you run into or search the QA for already posted solutions)

- Debian: https://docs.docker.com/install/linux/docker-ce/debian/

### 1. Installation Steps
	- Create Dockerhub account
		- https://hub.docker.com/signup

	- Install Docker
		- The Docker docs suggest setting up a Docker repository to install and update from.
		- This is where you should start: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

	- Login to the Dockerhub
		- In order to push and pull images, you will need to login to the Dockerhub. In your terminal, run docker login and then enter your Dockerhub account's username and password.

	- Test Docker installation
		- After completing the installation steps, test out Docker by running sudo docker run hello-world. This should download and run the test container printing "hello world" to your console.

	- Installing Docker Compose
		- Navigate to the installation page:
		- https://docs.docker.com/compose/install/#install-compose-as-standalone-binary-on-linux-systems
		- Click the Linux Standalone Binary Tab:

		- Then, CTL+F to search for the Install Compose as standalone binary on Linux systems section on the page. Follow the instructions provided there to install the latest Docker Compose binary.

	- Testing Docker Compose
		- After completing, test your installation by running docker-compose -v. This should print the version and build numbers to your console.

	- Run without Sudo
		- Follow these instructions to run Docker commands without sudo:
		- https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user

	- Start on Boot
		- Follow these instructions so that Docker and its services start automatically on boot:
		- https://docs.docker.com/install/linux/linux-postinstall/#configure-docker-to-start-on-boot

NOTE: You may need to restart your system before starting the course material.