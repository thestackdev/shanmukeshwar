---
title: "Setup docker on ubuntu"
date: 2023-04-23T18:21:57+05:30
draft: false
cover:
  image: "/blog/setup-docker-on-ubuntu.png"
  alt: "Setup docker on ubuntu"
  relative: false
---

### Step 1: Update Packages

Before installing any new packages, it is recommended to update the existing packages on your Ubuntu system. You can do this by running the following command:

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Required Packages

Next, you need to install some required packages that are necessary for Docker to work properly. Run the following command to install these packages:

```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg
```

### Step 3: Add Docker GPG Key

Docker provides a GPG key to sign their packages, which helps ensure the authenticity of the packages. To add the Docker GPG key, run the following commands:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### Step 4: Add Docker Repository

After adding the Docker GPG key, you need to add the Docker repository to your system. Run the following command to add the Docker repository:

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Step 5: Install Docker

Now that you have added the Docker repository, you can install Docker on your system. Run the following command to install Docker:

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Step 6: Verify Installation

Once the installation is complete, you can verify the installation by running the following command:

```bash
sudo docker run hello-world
```

This command will download a test image and run it in a container. If everything is working properly, you should see a message indicating that Docker is installed and working correctly.

That's it! You have now installed Docker on Ubuntu and are ready to start using it to build and run containerized applications.

## Uninstall Docker

If you need to uninstall Docker from your Ubuntu system, you can do so by running the following command to remove the packages:

```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```

This command will remove all the Docker-related packages from your system.

## Remove Docker Data

After uninstalling Docker, you may also want to remove the Docker data and configuration files. You can do this by running the following commands:

```bash
sudo rm -rf /var/lib/docker sudo rm -rf /var/lib/containerd
```

These commands will remove the Docker data and configuration files from your system.

## Additional Docker Commands

### Copy Docker Image Over SSH

If you need to copy a Docker image from one machine to another over SSH, you can use the `docker save` and `docker load` commands along with the `ssh` command. Run the following command to copy the Docker image:

```bash
docker save docker-image | ssh -C ubuntu@hostname docker load
```

This command will save the `docker-image` to a tar file, copy it over SSH, and load it on the remote machine.

### List and Kill All Containers

To list all Docker containers and kill them, you can use the `docker ps -a -q` command to list all container IDs and the `docker rm -f` command to remove them. Run the following command to list and kill all containers:

```bash
docker rm -f $(docker ps -a -q)
```

This command will remove all containers running or stopped.

### Execute QEMU Commands

If you need to execute `qemu` commands in a Docker container, you can use the `multiarch/qemu-user-static` image to run them. Run the following command to execute `qemu` commands:

```bash
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
```

This command will download and run the `multiarch/qemu-user-static` image, which includes the `qemu` emulator. The `--privileged` flag is used to give the container elevated privileges to execute `qemu` commands.

That's it! You have now successfully uninstalled Docker from your Ubuntu system and removed all its related data and configuration files.
