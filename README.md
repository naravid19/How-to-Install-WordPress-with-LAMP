## How to Install WordPress with LAMP

refer : https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lemp-on-ubuntu-22-04

- [Creating a MySQL Database and User for WordPress](#creating-a-mysql-database-and-user-for-wordpress)
- [Installing Additional PHP Extensions](#installing-additional-php-extensions)
- [Adjusting Apache’s Configuration to Allow for .htaccess Overrides and Rewrites](#adjusting-apaches-configuration-to-allow-for-htaccess-overrides-and-rewrites)
- [Enabling the Rewrite Module](#enabling-the-rewrite-module)
- [Enabling the Changes](#enabling-the-changes)
- [Downloading WordPress](#downloading-wordpress)
- [Configuring the WordPress Directory](#configuring-the-wordpress-directory)
    - [Setting Up the WordPress Configuration File](#setting-up-the-wordpress-configuration-file)

## Creating a MySQL Database and User for WordPress
    sudo mysql
    mysql -u root -p

Create the database for WordPress

    CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;

Create the user for WordPress

    CREATE USER 'wordpressuser'@'%' IDENTIFIED WITH mysql_native_password BY 'password';

    GRANT ALL ON wordpress.* TO 'wordpressuser'@'%';

    FLUSH PRIVILEGES;

    EXIT;

## Installing Additional PHP Extensions
    sudo apt update
    sudo apt install php8.2-curl
    sudo apt install php8.2-gd
    sudo apt install php8.2-mbstring
    sudo apt install php8.2-xml
    sudo apt install php8.2-xmlrpc
    sudo apt install php8.2-soap
    sudo apt install php8.2-intl
    sudo apt install php8.2-zip
    sudo systemctl restart apache2

## Adjusting Apache’s Configuration to Allow for .htaccess Overrides and Rewrites
    sudo nano /etc/apache2/sites-available/wordpress.conf

/etc/apache2/sites-available/wordpress.conf

    <VirtualHost *:80>
    . . .
    <Directory /var/www/wordpress/>
        AllowOverride All
    </Directory>
    . . .
    </VirtualHost>

## Enabling the Rewrite Module
    sudo a2enmod rewrite

## Enabling the Changes
    sudo apache2ctl configtest
    sudo systemctl restart apache2

## Downloading WordPress
    cd /tmp
    curl -O https://wordpress.org/latest.tar.gz
    tar xzvf latest.tar.gz
    touch /tmp/wordpress/.htaccess
    cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
    mkdir /tmp/wordpress/wp-content/upgrade
    sudo cp -a /tmp/wordpress/. /var/www/wordpress

## Configuring the WordPress Directory
    sudo chown -R www-data:www-data /var/www/wordpress
    sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \;
    sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \;

### Setting Up the WordPress Configuration File

    curl -s https://api.wordpress.org/secret-key/1.1/salt/
    sudo nano /var/www/wordpress/wp-config.php

you can explicitly set the filesystem method to “direct”. Failure to set this with your current settings would result in WordPress prompting for FTP credentials when performing some actions.

    define('FS_METHOD', 'direct');
