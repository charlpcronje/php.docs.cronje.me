---: Deploying Your Application | CRONje.ME
label: Deploying Application
order: 4
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
tags: [dev,tools,js,frontend,backend,devtools,php,laravel]
---
# Laravel 8 Vite Vue Project

[https://laravel-vite.innocenzi.dev/guide/](https://laravel-vite.innocenzi.dev/guide/_)

## Installation

## Via Laravel Installer.

First, download the Laravel installer using Composer.

```shell
composer global require "laravel/installer=~1.1"
```

Make sure to place the `~/.composer/vendor/bin directory in your PATH (or C:\%HOMEPATH%\AppData\Roaming\Composer\vendor\bin` if working with Windows) so the laravel executable is found when you run the laravel command in your terminal.

Once installed, the simple `laravel new` command will create a fresh Laravel installation in the directory you specify. For instance, `laravel new blog` would create a directory named `blog` containing a fresh Laravel installation with all dependencies installed. This method of installation is much faster than installing via Composer.

## Via Composer
The Laravel framework utilizes [Composer](`http://getcomposer.org/`) for installation and dependency management. If you haven't already, start by [0`installing Composer`](http://getcomposer.org/doc/00-intro.md)

Now you can install Laravel by issuing the following command from your terminal: