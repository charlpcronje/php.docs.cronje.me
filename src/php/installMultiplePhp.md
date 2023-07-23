---
title: PHP Multiple Versions on same server | CRONje.ME
label: PHP Multiple Versions
order: 26
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
tags: [dev,extension,chrome,manifest,v3,UNOCSS]
---

# Install Multiple Versions of PHP it the same time on Rocky Linux 8.7

To install `PHP 7.4` and `PHP 8.2.6` on the same server and run different virtual hosts on different versions on` Rocky Linux 8.7`, you will need to follow the steps below:

Install `PHP 7.4` and `PHP 8.2.6`: Rocky Linux offers the ability to install multiple PHP versions on your system using the `Remi PHP` repository
## Install the Remi repository

```sh
sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

## Install PHP 7.4

```sh
sudo dnf module reset php
sudo dnf module enable php:remi-7.4
sudo dnf install php php-cli php-fpm php-mysqlnd php-zip php-devel php-gd php-mcrypt php-mbstring php-curl php-xml php-pear php-bcmath php-json
```

## Install PHP 8.2.6

```sh
sudo dnf module reset php
sudo dnf module enable php:remi-8.2
sudo dnf install php php-cli php-fpm php-mysqlnd php-zip php-devel php-gd php-mcrypt php-mbstring php-curl php-xml php-pear php-bcmath php-json
```

You will also need to specify the default PHP version for the system using update-alternatives tecmint.com.

## Set PHP 8.2.6 as the default version

```sh
sudo update-alternatives --set php /usr/bin/php8.2
```

## What if I already have PHP version 8.1.17 or some other version Installed

You don't need to remove your current PHP version `(8.1.17)` before installing `PHP 7.4` and P`HP 8.2.6` on `Rocky Linux 8.7`. The `Remi repository`, which we're using to install the `PHP` versions, allows for multiple `PHP` versions to be installed and used concurrently on the same system

However, you need to be aware that the `dnf` package manager, which is used to install `PHP in Rocky Linux`, uses the concept of `module streams` to provide different versions of packages. When you enable a specific PHP module stream (like `php:remi-7.4` or `php:remi-8`.
2), the system sets that version as the default version for PHP-related commands (like `php` or `php-fpm`)

If you want to keep `PHP 8.1.17` as the default version for the system while also being able to use `PHP 7.4` and `PHP 8.2.6`, you can do so by not running the dnf module enable command for `PHP 7.4` and `PHP 8.2.6`. Instead, you can install these versions directly using the dnf install command with the specific package names for these versions:

## Install PHP 7.4

```sh
sudo dnf install php74 php74-php-cli php74-php-fpm php74-php-mysqlnd php74-php-zip php74-php-devel php74-php-gd php74-php-mcrypt php74-php-mbstring php74-php-curl php74-php-xml php74-php-pear php74-php-bcmath php74-php-json
```

## Install PHP 8.2.6

```sh
sudo dnf install php82 php82-php-cli php82-php-fpm php82-php-mysqlnd php82-php-zip php82-php-devel php82-php-gd php82-php-mcrypt php82-php-mbstring php82-php-curl php82-php-xml php82-php-pear php82-php-bcmath php82-php-json
```

The above commands will install `PHP 7.4` and `PHP 8.2.6` with their command-line interface (`CLI`) and FastCGI Process Manager (`FPM`) without changing the default `PHP` version on the system. You can then use the specific commands (`php74, php74-php-fpm, php82, php82-php-fpm`) to run these versions when needed.

Remember to adjust your `PHP-FPM` and `Apache` configurations to use the correct `PHP` versions and commands as per your requirements


Configure `PHP-FPM `for each `PHP version`: Each `PHP` version needs its own `PHP-FPM` instance running on a different port

## Configure `PHP 8.2.6 FPM`

```sh
sudo nano /etc/php-fpm.d/www.conf
```

## Change the listen directive to: `listen = 9001`

### Configure `PHP 7.4 FPM`

```sh
sudo vim /etc/opt/remi/php74/php-fpm.d/www.conf
```

## Change the listen directive to: `listen = 9002`

## Start PHP-FPM services

```sh
sudo systemctl start php-fpm
sudo systemctl start php74-php-fpm
```

Configure Apache virtual hosts to use different PHP versions: Each virtual host needs to be configured to use a different PHP-FPM instance stackoverflow.com.

### Edit virtual host for website1

```sh
sudo vim /etc/httpd/conf.d/website1.conf 
```

### Add the following directive within the VirtualHost block

```conf
<FilesMatch \.php$>
    # PHP 8.2.6
    SetHandler "proxy:fcgi://127.0.0.1:9001"
</FilesMatch>
```

### Edit virtual host for website2

```sh
sudo vim /etc/httpd/conf.d/website2.conf
```

### Add the following directive within the VirtualHost block

```conf
<FilesMatch \.php$>
    # PHP 7.4
    SetHandler "proxy:fcgi://127.0.0.1:9002"
</FilesMatch>
```

### Restart Apache

```sh
sudo systemctl restart httpd
```

Enable necessary Apache modules: Enable several modules so that your Apache service can work with multiple PHP versions

```sh
sudo a2enmod actions fcgid alias proxy_fcgi
sudo systemctl restart apache2
```

After these steps, you should have both `PHP 7.4` and `PHP 8.2.6` installed on your server. Each version should be running in its own PHP-FPM instance, and each virtual host should be configured to use a different PHP version.


## vhost file entry example

To declare a virtual host for a specific PHP version in the vhost.conf file, you will need to configure the VirtualHost block in the Apache configuration file. This configuration block will define the settings for a specific domain.

In this block, you will specify the version of PHP that this virtual host should use by adding a FilesMatch block that sets the handler to the appropriate `PHP-FPM` instance.

Here's an example of how you might configure a virtual host to use `PHP 7.4`:

```conf
<VirtualHost *:80>
    ServerName website2.com
    DocumentRoot "/var/www/website2"

    <Directory "/var/www/website2">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        # PHP 7.4
        SetHandler "proxy:fcgi://127.0.0.1:9002"
    </FilesMatch>

    ErrorLog ${APACHE_LOG_DIR}/website2.com.error.log
    CustomLog ${APACHE_LOG_DIR}/website2.com.access.log combined
</VirtualHost>
```

### In this configuration:

- *ServerName:* is the domain name that this virtual host will respond to.
- *DocumentRoot:* is the directory where the website's files are stored.
- The *Directory* block contains settings that apply to the files in the specified directory. In this case, it allows Apache to follow symbolic links and overrides other configuration settings.
- The `FilesMatch` block with the `SetHandler` directive is used to forward PHP requests to the `PHP 7.4 FPM` instance running on port `9002`.
- `ErrorLog` and `CustomLog` are set to specify where the server will log errors and access requests for this virtual host.

After making these changes, you should restart the Apache service to apply the changes:

```sh
sudo systemctl restart httpd
```

This configuration will tell Apache to use PHP 7.4 for requests to `website2.com` 

Repeat this process for each virtual host, adjusting the `ServerName`, `DocumentRoot`, and `PHP-FPM` port as needed.

Remember to check your configuration files for any syntax errors using `apachectl configtest` before restarting the service