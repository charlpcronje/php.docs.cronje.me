---: Serving Laravel | CRONje.ME
label: Serving Laravel
order: 6
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
tags: [dev,tools,js,frontend,backend,devtools,php,laravel]
---Typically, you may use a web server such as Apache or Nginx to serve your Laravel applications. If you are on PHP 5.4+ and would like to use PHP's built-in development server, you may use the `serve` Artisan command:

```shell
php artisan serve
```

By default the HTTP-server will listen to port 8000\. However if that port is already in use or you wish to serve multiple applications this way, you might want to specify what port to use. Just add the --port argument:

```shell
php artisan serve --port=8080
```
