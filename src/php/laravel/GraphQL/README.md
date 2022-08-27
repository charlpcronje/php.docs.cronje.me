---: Laravel Lighthouse | GraphQL
label: Laravel Lighthouse
order: 0
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
tags: [dev,tools,js,frontend,backend,devtools,php,laravel]
---
# [Installation](https://lighthouse-php.com/5/getting-started/installation.html#ide-support)

The following section teaches you how to install Lighthouse in your project.

## Install via composer

```shell
composer require nuwave/lighthouse
```

## Publish the default schema

Lighthouse includes a default schema to get you going right away. Publish it using the following artisan command:

```php
php artisan vendor:publish --tag=lighthouse-schema
```

## Lumen

```php
Register the service provider in your bootstrap/app.php file:
$app->register(\Nuwave\Lighthouse\LighthouseServiceProvider::class);
```

The many features Lighthouse provides are split across multiple service providers. Since Lumen does not support auto-discovery, you will have to register them individually depending on which features you want to use. Check Lighthouse's composer.json (opens new window), the section extra.laravel.providers contains the default service providers.

## IDE Support

Lighthouse makes heavy use of the SDL and utilizes schema directives. To improve your editing experience, you can generate a definition file with an artisan command:

```php
php artisan lighthouse:ide-helper
We recommend the following plugins:
```

### IDE	Plugin

PhpStorm    [https://plugins.jetbrains.com/plugin/8097-js-graphql](https://plugins.jetbrains.com/plugin/8097-js-graphql)

## Install GraphQL DevTools

To make use of the amazing tooling around GraphQL, we recommend installing GraphQL Playground (opens new window).

`composer require mll-lab/laravel-graphql-playground`

After installation, visit `/graphql-playground` to try it.

You can use any GraphQL client with Lighthouse, make sure to point it to the URL defined in the config. By default, the endpoint lives at `/graphql`.