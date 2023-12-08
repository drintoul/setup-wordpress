# wordpress
Collection of steps to create a self-hosted Wordpress site

## Create Server

Get Ubuntu Server 22.04 from https://ubuntu.com/download/server
<ul>
  <li>Install Ubuntu Server 22.04 in virtual machine
  <li>Make sure to enable SSH server
</ul>

## Update

sudo apt update && sudo apt upgrade -y

https://vsys.host/how-to/how-to-install-wordpress-on-ubuntu-22-04-with-a-lamp-stack

## Install Apache Web Server

<ul>
  <li>sudo apt install apache2 -y
  <li>sudo systemctl start apache2
  <li>sudo systemctl status apache2
</ul>

## Install MySQL

<ul>
  <li>sudo apt install mysql-server -y
  <li>sudo mysql_secure_installation
  <li>set root password, remove anonymous users, disable remote login, remove test database
</ul>

## Install PHP and required modules

<ul>
  <li>sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-imagick php-mbstring php-xml php-xmlrpc -y
  </li>sudo systemctl restart apache2
</ul>

## Create MySQL database and user for WordPress

sudo mysql -u root -p

<ul>
  <li>CREATE DATABASE wordpress;
  <li>CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'password';
  <li>GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
  <li>FLUSH PRIVILEGES;
  <li>quit
</ul>

## Install WordPress

<ul>
  <li>cd /var/www/html
  <li>sudo wget -c http://wordpress.org/latest.tar.gz
  <li>sudo tar -xzvf latest.tar.gz
  <li>cd wordpress
  <li>sudo cp wp-config-sample.php wp-config.php
</ul>

sudo nano wp-config.php

<ul>
  <li>define('DB_NAME', 'wordpress');
  <li>define('DB_USER', 'wpuser');
  <li>define('DB_PASSWORD', 'password');
</ul>

## Change Permissions

https://wordpress.stackexchange.com/questions/19649/wordpress-on-localhost-lamp-doesnt-let-me-install-plugins

<ul>
  <li>sudo chown -R www-data:www-data /var/www/html
  <li>sudo chmod -R g+rwX /var/www/html
</ul>

## Increase Max Upload Filesize

https://servebolt.com/articles/increase-maximum-upload-file-size-in-wordpress/

<ul>
  <li>sudo nano /etc/php/8.1/apache2/php.ini

  <li>put the following at bottom of file:

  <ul>
    <li>upload_max_filesize = 100M
    <li>post_max_size = 200M
    <li>memory_limit = 128M
  </ul>
</ul>

## Change default Site for Apache

<ul>
  <li>sudo nano /etc/apache2/sites-enabled/000-default.conf
  <li>append /wordpress to DocumentRoot
  <li>sudo systemctl restart apache2
</ul>

## Install SSL Certificate

https://certbot.eff.org/instructions?ws=apache&os=ubuntufocal

<ul>
  <li>sudo snap install --classic certbot
  <li>sudo ln -s /snap/bin/certbot /usr/bin/certbot
  <li>sudo certbot --apache
</ul>
