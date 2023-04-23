---
title: "Things to do after installing Ubuntu Server"
date: 2023-04-23T18:21:57+05:30
draft: false
---

![Things to do after installing Ubuntu Server](/blog/things-to-do-after-installing-ubuntu.png)

When setting up a new Ubuntu 20.04 server, it's important to perform some essential configuration steps to increase its security and usability. These steps will give you a solid foundation for subsequent actions on your server. In this article, we'll cover the important configuration steps for an Ubuntu 20.04 server, and provide some guidance on how to use and maintain the server after completing the initial setup.

### Step 1: Logging in as root

When you first log in to your server as root, you should update the server's package index and upgrade any installed packages to their latest versions. This can be done using the following commands

```bash
sudo apt update
sudo apt upgrade -y
```

Updating the server's packages ensures that you have the latest security updates and bug fixes installed.

### Step 2: Creating a new user

Create a new user account with a strong password that will be used for day-to-day use. You'll be prompted to enter a few details, starting with the account password. Once the user account has been created, you should add it to the sudo group to grant administrative privileges to the new user. This can be done using the following command

```bash
sudo usermod -aG sudo username
```

Replace "username" with the username you created in Step 2.

### Step 3: Granting administrative privileges

To avoid having to log out of the new user and log back in as the root account, you can set up superuser or root privileges for the new user. This will allow the new user to run commands with administrative privileges by putting the word "sudo" before the command.

### Step 4: Setting up a basic firewall

Ubuntu 20.04 servers can use the UFW firewall to make sure only connections to certain services are allowed. You can set up a basic firewall using this application to enhance the security of your server. Some common UFW commands include

```bash
sudo ufw enable
sudo ufw status
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
```

### Step 5: Enabling external access for your regular user

You can enable SSH access for your new user account by using SSH with your new username. You can also enhance your server's security by setting up SSH keys instead of using password authentication.

### Step 6: Removing snap packages (optional)

Snap is a package management system used in Ubuntu to install and manage software applications. However, it can take up a lot of disk space and slow down your system. If you want to remove snap packages from your Ubuntu system, you can use the following command

```bash
sudo snap list | awk '{print $1}' | xargs sudo snap remove -y
```

### Step 7: Completely removing snap (optional)

If you want to completely remove snap from your Ubuntu system, you can use the following command

```bash
sudo apt purge snapd -y
```

### Step 8: Updating and cleaning the system

It's important to keep your system up-to-date with the latest software updates and security patches. To update your system, run the following command in the terminal

```bash
sudo apt update
sudo apt upgrade -y
```

You can also clean up your system using the following commands

```bash
sudo apt autoremove -y     # Remove any unused packages
sudo apt autoclean -y      # Remove any old package versions
```

Using the Server Now that you have completed the initial configuration steps, you can start
