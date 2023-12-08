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

sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl status apache2

## Install MySQL

sudo apt install mysql-server
sudo mysql_secure_installation
set root password, remove anonymous users, disable remote login, remove test database

## Install PHP and required modules

sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-imagick php-mbstring php-xml php-xmlrpc -y
sudo systemctl restart apache2

## Create MySQL database and user for WordPress

sudo mysql -u root -p

  CREATE DATABASE wordpress;
  CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'password';
  GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
  FLUSH PRIVILEGES;
  quit

## Install WordPress

cd /var/www/html
sudo wget -c http://wordpress.org/latest.tar.gz
sudo tar -xzvf latest.tar.gz
cd wordpress
sudo cp wp-config-sample.php wp-config.php

sudo nano wp-config.php

  define('DB_NAME', 'wordpress');
  define('DB_USER', 'wpuser');
  define('DB_PASSWORD', 'password');

sudo chown -R www-data:www-data /var/www/html
sudo chmod -R g+rwX /var/www/html

## Change Permissions

https://wordpress.stackexchange.com/questions/19649/wordpress-on-localhost-lamp-doesnt-let-me-install-plugins

## Increase Max Upload Filesize

https://servebolt.com/articles/increase-maximum-upload-file-size-in-wordpress/

sudo nano /etc/php/8.1/apache2/php.ini

put the following at bottom of file:

  upload_max_filesize = 100M
  post_max_size = 200M
  memory_limit = 128M

## Change default Site for Apache

sudo nano /etc/apache2/sites-enabled/000-default.conf

append /wordpress to DocumentRoot

sudo systemctl restart apache2

## Install SSL Certificate

https://certbot.eff.org/instructions?ws=apache&os=ubuntufocal

sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --apache
###
