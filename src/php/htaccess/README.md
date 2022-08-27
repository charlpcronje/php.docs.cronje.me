# SIMPLIFIED ENVORONMENT VARIABLE SETTINGS

- [SIMPLIFIED ENVORONMENT VARIABLE SETTINGS](#simplified-envoronment-variable-settings)
  - [1. .htacesss](#1-htacesss)
    - [1.1 Example](#11-example)
  - [2. Retreiving Environment Variables from .htaceess](#2-retreiving-environment-variables-from-htaceess)
  - [3. Input Class](#3-input-class)

## 1. .htacesss

> a .htacesss file can be craated anywhere in the file structure.

- The rules specified in each file be valid for the current directory and below.

### 1.1 Example

```shell
.. Root Folder
.htaccess (Rules applies for all files and folders, that includes all files and folders inside Model and model/phpmmailer)
  /root/model
     ..htaccess (Rules for this file applies for the Model directory and phpmailer etc. But it doen not delete any the parent .htaccess rules, only the same rukles that is defined again will override tha parent's rules.)
  /root/model/phpmailer
```

So this means we can specify the `default` values for the `DB connection` and `E-Mail settings` in the `document root's` `.htaccess` file and override those settings with values set in the `.htaccess` file in the `model folder`. The `.htaccess` file in the `document roo`t can be added to the the `SVN` repo but the `.htaccess` file in the `model folder` will be excluded from the `repo`

## 2. Retreiving Environment Variables from .htaceess

> PHP let you do this in a variaty of ways

- Using the `getenv('setting_key')` function
- Using the `super` global `$_ENV['setting_key']` variable
- Using the `filter_input(INPUT_TYPE,'setting_key',FLAG)` function

The `filter_input()` is the preferred wat because first you can specify the `origin` if the variable. INPUT_ENV, INPUT_SERVER, INPUT_GET, INPUT_POST. This stops someone from adding a setting with `$_ENV['setting'] = 'value'` and the using it as a `ENV` variable. The `filter_input()` retreive `null` for all `key => value` pairs being retreived of the wrong `INPUT_TYPE`. The second argument is the `key` of the setting you want to retreive. The 3rd argument you can choose between a lot of `PHP FLAGS` that will `validate` and `sanitize` the `inputs`.

## 3. Input Class

The Input class contains methods for gettting an integer, string, bool and email by calling the following static methods that don't actuallt exits:

```php
Env::int('key'); Env::str('key'); Env::bool('key'); Env::email('key');
Post::int('key'); Post::str('key'); Post::bool('key'); Post::email('key');
Get::int('key'); Get::str('key'); Get::bool('key'); Get::email('key');
Server::int('key'); Server::str('key'); Server::bool('key'); Server::email('key');
```

By calling these method the `__callStati`c Method will be called, which will intern `instantiate` the `Env`,`Post`,`Get`,`Server` class that extend the Input class and and run the corrosponding `_int` ,`_str`, `_bool`, `_email` method andreturn the value of the key given
