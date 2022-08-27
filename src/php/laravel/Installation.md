---: Installation | CRONje.ME
label: Installation
order: 10
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
tags: [dev,tools,js,frontend,backend,devtools,php,laravel]
---
# Installation

## Via Laravel Installer

First, download the Laravel installer using Composer.

```shell
composer global require "laravel/installer=~1.1"
```

Make sure to place the `~/.composer/vendor/bin` directory in your PATH (or `C:\%HOMEPATH%\AppData\Roaming\Composer\vendor\bin` if working with Windows) so the laravel executable is found when you run the laravel command in your terminal.

Once installed, the simple `laravel new` command will create a fresh Laravel installation in the directory you specify. For instance, `laravel new blog` would create a directory named `blog` containing a fresh Laravel installation with all dependencies installed. This method of installation is much faster than installing via Composer.

## Via Composer

The Laravel framework utilizes [Composer](http://getcomposer.org/) for installation and dependency management. If you haven't already, start by [installing Composer](http://getcomposer.org/doc/00-intro.md).

Now you can install Laravel by issuing the following command from your terminal:

```shell
composer create-project laravel/laravel your-project-name 4.2.*
```

This command will download and install a fresh copy of Laravel in a new `your-project-name` folder within your current directory.

If you prefer, you can alternatively download a copy of the [Laravel repository from GitHub](https://github.com/laravel/docs/archive/4.2.zip) manually. Next run the `composer install` command in the root of your manually created project directory. This command will download and install the framework's dependencies.
