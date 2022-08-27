---
title: Laravel 8 CORS | CRONje.ME
label: Laravel 8 CORS
order: 20
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
tags: [dev,tools,start,js,php,frontend,backend,developer,devtools,helpers,log]
---

Cross-origin resource sharing (CORS) is a mechanism that allows restricted resources on a web page to be requested from another domain outside the domain from which the first resource was served.

## CORS Middleware

Along with new app installation, config/cors.php file also generated. Laravel allows following cors related configurations.

- CORS configuration paths
- Allowed methods
- Allowed origins patterns
- Allowed headers
- Exposed headers
- Max age
- Supports credentials

```php
return [

    /*
    |--------------------------------------------------------------------------
    | Cross-Origin Resource Sharing (CORS) Configuration
    |--------------------------------------------------------------------------
    |
    | Here you may configure your settings for cross-origin resource sharing
    | or "CORS". This determines what cross-origin operations may execute
    | in web browsers. You are free to adjust these settings as needed.
    |
    | To learn more: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
    |
    */
    'paths' => ['api/*'],
    'allowed_methods' => ['*'],
    'allowed_origins' => ['*'],
    'allowed_origins_patterns' => [],
    'allowed_headers' => ['*'],
    'exposed_headers' => [],
    'max_age' => 0,
    'supports_credentials' => false,
];
```

## Malevolent Laravel CORS Error

No `Access-Control-Allow-Origin` header is present on the requested resource.), it occurred because we have two different domain trying to exchange data with each other. And yes we haven’t even enabled the `CORS` yet.

```error
Access to XMLHttpRequest at `http://localhost:8000/demo-url` from origin 'null' 
has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is 
present on the requested resource.
```

The best thing with CORS for dev is to just add wildcards for all he CORS rules:

```php
# config/cors.php

return [

    /*
    |--------------------------------------------------------------------------
    | Cross-Origin Resource Sharing (CORS) Configuration
    |--------------------------------------------------------------------------
    |
    | Here you may configure your settings for cross-origin resource sharing
    | or "CORS". This determines what cross-origin operations may execute
    | in web browsers. You are free to adjust these settings as needed.
    |
    | To learn more: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
    |
    */


    'paths' => ['*'],                           // `/api` or `/client`..

    'allowed_methods' => ['*'],                 // GET AND POST works nby default but the rest of the restFull ones does not( Put , patch...)

    'allowed_origins' => ['*'],                 // Comma separated list of origins

    'allowed_origins_patterns' => ['*'],        // Regex of domains or origins that can make requests

    'allowed_headers' => ['*'],                 // This is where i'm stuck now between this one and the next one, for now its just a wildcard until I need to look this up

    'exposed_headers' => ['*'],                 // Specify any custom headers you are using

    'max_age' => 0,                             // The browser does not have to check the rules with every request. But whe it is set to 0 it will, otherwise you can change that value to 60 and it will only check if there were any changes every time

    'supports_credentials' => false             // Does i require you to have auth credentials in the header to be able to request the resource
];
```

`CORS` has left me stumped more than once but finally it is now standard on Laravel 7+ and you can just use the wildcard. So problem solved :)

`PS: Before you go to production you better fix CORS`
