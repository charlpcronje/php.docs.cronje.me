---
title: PHP Preload | CRONje.ME
label: PHP Preload
order: 25
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
tags: [dev,extension,chrome,manifest,v3,UNOCSS]
---

As of `PHP 7.4.0`, `PHP` can be configured to preload scripts into the `opcache` when the engine starts. Any functions, classes, interfaces, or traits (but not constants) in those files will then become globally available for all requests without needing to be explicitly included. That trades convenience and performance (`because the code is always available`) for baseline memory usage. It also requires restarting the `PHP process` to clear `pre-loaded` scripts, meaning this feature is only practical to use in production, not in a development environment.

Note that the optimal `tradeoff` between performance and memory may vary with the application. "`Preload everything`" may be the easiest strategy, but not necessarily the best strategy. Additionally, preloading is only useful when there is a persistent process from one request to another. That means while it can work in a `CLI script` if the `opcache` is enabled, it's generally pointless. The exception is when using preloading

> **Note**
> Preloading is not supported on Windows.

Configuring preloading involves two steps, and requires that the opcache be enabled. First, set the opcache.preload value in `php.ini`:

```ini
opcache.preload=preload.php
```

preload.php is an arbitrary file that will run once at server startup (PHP-FPM, mod_php, etc.) and load code into persistent memory. If PHP will be run as root (not recommended), the opcache.preload_user value can specify an alternate system user to run the preloading. Running preloading as root is not allowed.

In the `preload.php` script, any file referenced by `include`, `include_once`, `require`, `require_once`, or `opcache_compile_file()` will be parsed into persistent memory. In the following example, all .php files in the src directory will be preloaded, unless they are a Test file.

```php
$directory = new RecursiveDirectoryIterator(__DIR__ . '/src');
$fullTree = new RecursiveIteratorIterator($directory);
$phpFiles = new RegexIterator($fullTree, '/.+((?<!Test)+\.php$)/i', RecursiveRegexIterator::GET_MATCH);

foreach ($phpFiles as $key => $file) {
    require_once($file[0]);
}
?>
```

Both `include` and `opcache_compile_file()` will work, but have different implications for how code gets handled.

- `include` will execute code in the file, while `opcache_compile_file()` will not. That means only the former supports conditional declaration (`functions declared inside an if-block`).
- Because include will execute code, nested included files will also be parsed and their declarations `preloaded`.
- `opcache_compile_file()` can load files in any order. That is, if `a.php` defines class A and `b.php` defines `class B` that extends A, then opcache_compile_file() can load those two files in any order. When using include, however, `a.php` must be included first.
- In either case, if a later script includes a file that has already been preloaded then its contents will still execute, but any symbols it defines will not be re-defined. Using `include_once` will not prevent the file from being included a second time. It may be necessary to load a file again in order to include global constants defined in it, as those are not handled by preloading.
