---
title: "Setup Nextcloud on Ubuntu"
date: 2023-04-23T18:21:57+05:30
draft: false
---

## Initial server setup

### Adding a user

After setting up Ubuntu Server, create a user for yourself if you don’t already have one:

```
adduser <username>
```

Adding the user to the sudo group:

```
sudo usermod -aG sudo <username>
```

After creating your user, be sure to log out from root, log in as that user.

### Updating packages

Before continuing, let’s make sure all installed packages are up to date.

```
sudo apt update
```

```
sudo apt dist-upgrade
```

Clean up orphan packages (if there are any):

```
sudo apt autoremove
```

### Updating the hostname

Edit the following files, and be sure they include the proper hostname or domain name for your server:

```
sudo nano /etc/hostname
```

```
sudo nano /etc/hosts
```

Reboot your server so that all the changes we’ve made so far will take effect.

```
sudo reboot
```

While that’s rebooting, update DNS for the domain name if you have one, so that can replicate while we finish the other steps.

### Downloading Nextcloud

We’ll need to grab the Nextcloud zip file, which contains the necessary files we’ll be needing. Click [here](https://nextcloud.com/install/?ref=blog.codefusionz.com) to open the download page, then copy the URL for the zip file.

On the server, download the Nextcloud zip file using the URL that you copied from the site:

```
wget https://download.nextcloud.com/server/releases/latest.zip
```

Note: If that URL doesn’t work (it can change at any time) grab the URL from the [Nextcloud site](https://nextcloud.com/install/?ref=blog.codefusionz.com#instructions-server).

## MariaDB Setup

### Setting up the database server

First, let’s install the mariadb-server package:

```
sudo apt install mariadb-server
```

Check the status of the `mariadb` service:

```
systemctl status mariadb
```

### Running the secure installation script

Although there’s many tweaks and adjustments you can make to secure MariaDB, running the following command and answering the prompts will give us a decent starting point:

```
sudo mysql_secure_installation
```

Follow the prompts to set up some very basic security defaults for the database server.

### Creating the Nextcloud Database

Next, we’ll create the database we’ll be using for Nextcloud. To do this, we’ll need to access the MariaDB console:

```
sudo mariadb
```

Then, we’ll create the database and set up permissions with the following commands:

```
CREATE DATABASE nextcloud;
```

```
SHOW DATABASES;
```

```
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost' IDENTIFIED BY 'mypassword';
```

```
FLUSH PRIVILEGES;
```

CTRL+D to exit

## Apache Webserver Setup

Installing the required packages to support Apache:

```
sudo apt install php php-apcu php-bcmath php-cli php-common php-curl php-gd php-gmp php-imagick php-intl php-mbstring php-mysql php-zip php-xml
```

Check the status with of Apache:

```
systemctl status apache2
```

Enable the recommended PHP extensions:

```
sudo phpenmod bcmath gmp imagick intl
```

Install zip and unzip the Nextcloud zip file:

```
sudo apt install unzip
```

```
unzip latest.zip
```

Now that we’ve unzipped the files, let’s move the files to where they’ll be served from and also set the permissions as well:

```
mv nextcloud nextcloud
```

```
sudo chown -R www-data:www-data nextcloud
```

```
sudo mv nextcloud /var/www
```

```
sudo a2dissite 000-default.conf
```

### Creating a host configuration file for Nextcloud

Next, we’ll set up a config file for Apache that tells it how to serve Nextcloud.

```
sudo nano /etc/apache2/sites-available/nextcloud.conf
```

Add the following contents to the file (be sure to adjust the file names to match yours):

```
<VirtualHost *:80>
    DocumentRoot "/var/www/nextcloud"
    ServerName nextcloud

    <Directory "/var/www/nextcloud/">
        Options MultiViews FollowSymlinks
        AllowOverride All
        Order allow,deny
        Allow from all
   </Directory>

   TransferLog /var/log/apache2/nextcloud_access.log
   ErrorLog /var/log/apache2/nextcloud_error.log

</VirtualHost>
```

Enable the site:

```
sudo a2ensite apache-config-file-name.conf
```

## Configuring PHP

Almost there! The next step will have us change some PHP options. First, edit the following file:

```
sudo nano /etc/php/8.1/apache2/php.ini
```

Adjust the following parameters:

```
memory_limit = 512M
upload_max_filesize = 200M
max_execution_time = 360
post_max_size = 200M
date.timezone = America/Detroit
opcache.enable=1
opcache.interned_strings_buffer=8
opcache.max_accelerated_files=10000
opcache.memory_consumption=128
opcache.save_comments=1
opcache.revalidate_freq=1
```

Enable the following PHP mods for Apache:

```
sudo a2enmod dir env headers mime rewrite ssl
```

Restart Apache to ensure the new PHP settings take effect:

```
sudo systemctl restart apache2
```

## Acquiring a TLS certificate

Let’s set up Let’s Encrypt and obtain a certificate for our Nextcloud installation. The following steps will guide you through the process.

Note: Instructions are taken from [this link](https://certbot.eff.org/instructions?ws=apache&os=ubuntufocal&ref=blog.codefusionz.com), which you may want to visit in case the instructions change in the future.

Ensure `snapd` is installed:

```
sudo apt install snapd
```

Install the core snap:

```
sudo snap install core; sudo snap refresh core
```

Install Certbot:

```
sudo snap install --classic certbot
```

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

Attempt to obtain a certificate (DNS must have already propagated):

```
sudo certbot --apache
```

Answer the prompts carefully, and as long as you didn’t overlook everything you should have your very own TLS certificate!

## Misc. Tweaks and Adjustments

### Correct the permissions of the config.php file

We definitely wouldn’t want the `config.php` file to fall into the wrong hands, as it contains valuable setup information regarding our Nextcloud setup. Let’s adjust the permissions to better protect it.

```
sudo chmod 660 /var/www/<nextcloud_directory>/config/config.php
```

```
sudo chown root:www-data /var/www/<nextcloud_directory/config/config.php
```

### Enable memory caching

Edit the Nextcloud config file:

```
sudo vim /var/www/nextcloud/config/config.php
```

Add the following line to the bottom:

```
'memcache.local' => '\\OC\\Memcache\\APCu',
```

### Resolving warnings pertaining to the default phone region

Edit the Nextcloud config file:

```
sudo vim /var/www/nextcloud/config/config.php
```

Add the following line to the bottom of the file:

```
'default_phone_region' => 'US',
```

Note: Be sure to change “US” in the above example to your two-character country code, if yours is not `US`.

### Get rid of the Image Magick error

Install the libmagickcore-6.q16-6-extra package:

```
sudo apt install libmagickcore-6.q16-6-extra
```

### Enabling Strict Transport Security

Edit the SSL config file for our Nextcloud installation:

```
sudo vim /etc/apache2/sites-available/nextcloud-le-ssl.conf
```

Add the following line after the `ServerName` line:

```
<IfModule mod_headers.c>
    Header always set Strict-Transport-Security "max-age=15552000; includeSubDomains"
</IfModule>
```
