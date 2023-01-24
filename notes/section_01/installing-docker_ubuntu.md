# Installing Docker on Ubuntu

Sources: 

- https://docs.docker.com/engine/install/ubuntu/
- https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

### - Install using the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

### 1. Set up the repository

- Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:

```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

- Add Docker’s official GPG key:

```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

- Use the following command to set up the repository:

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 2. Install Docker Engine

- Update the `apt` package index:

```
sudo apt-get update
```

NOTE: Receiving a GPG error when running apt-get update?

Your default umask may be incorrectly configured, preventing detection of the repository public key file. Try granting read permission for the Docker public key file before updating the package index:

```
sudo chmod a+r /etc/apt/keyrings/docker.gpg
sudo apt-get update
```

- Install Docker Engine, containerd, and Docker Compose.

[Latest]
To install the latest version, run:

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

[Specific Version]
To install a specific version of Docker Engine, start by list the available versions in the repository:

```
# List the available versions:
apt-cache madison docker-ce | awk '{ print $3 }'

5:20.10.16~3-0~ubuntu-jammy
5:20.10.15~3-0~ubuntu-jammy
5:20.10.14~3-0~ubuntu-jammy
5:20.10.13~3-0~ubuntu-jammy
```

Select the desired version and install:

```
VERSION_STRING=5:20.10.13~3-0~ubuntu-jammy
sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-compose-plugin
```

- Verify that the Docker Engine installation is successful by running the hello-world image:

```
sudo docker run hello-world
```

NOTE: If you receive this type of error, you may need to uninstall `apparmor` from the LXC container

https://newspaint.wordpress.com/2021/01/31/docker-apparmor-enabled-but-default-profile-could-not-be-loaded/

```
jonathan@dockerhost-02:~$ docker run hello-world
docker: Error response from daemon: AppArmor enabled on system but the docker-default profile could not be loaded: running `/usr/sbin/apparmor_parser apparmor_parser -Kr /var/lib/docker/tmp/docker-default3912811045` failed with output: apparmor_parser: Unable to replace "docker-default".  Permission denied; attempted to load a profile while confined?

error: exit status 243.
```

Solve:

```
sudo apt remove apparmor
```