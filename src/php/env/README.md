# ENV support

- [ENV support](#env-support)
  - [Description](#description)
    - [Repo Updates](#repo-updates)
    - [Setting ENV to `dev` | `test` | `live` in virtual hosts `.conf`](#setting-env-to-dev--test--live-in-virtual-hosts-conf)
    - [Default or missing settings](#default-or-missing-settings)
    - [Some optimization](#some-optimization)
    - [Some final things to test](#some-final-things-to-test)

## Description

Add a `.env` file to the developemnt stream to contain all settings required for that respective stream. The the case of the `website` this will be the `email`, `database`, `logs` and `paths` settings

### Repo Updates

- The structure and default values will be added to `.default.env`
- The `.default.env` file will be added to the repo
- PHP class will added to the repo that parses the `.env` file
- Name Spacing will be used to ensure the correct settings are used per environment
- if the `.env` file does not exist it will be created with the default values

### Setting ENV to `dev` | `test` | `live` in virtual hosts `.conf`

```conf
<VirtualHost *:80>
    ServerName www.default.com
    ServerAlias default.com

    SetEnv APP_ENV 'dev'

    DocumentRoot /var/www/default.com/public_html
    ErrorLog /var/www/default.com/error.log
    CustomLog /var/www/default.com/requests.log combined

</VirtualHost>
```

> .default.env

```conf
# EMAIL HOST SETTINGS
# default
default.email.smtp.host = hostname
default.email.smtp.pass = password
```

> .env (dev env)

```conf
# EMAIL HOST SETTINGS
# dev
dev.email.smtp.host = hostname
dev.email.smtp.pass = password
```

### Default or missing settings

- To define the structure and defaults for the settings go in `default.env` file will be created and added to the repo.
- The repo will not contain the `.env` file.
- If no `.env` file is found then the `default.env` file is copied to `.env` (`.env` will take priority over `default.env` 
- If settings get added to `default.env` will be added to `.env`

First the system loads  `defaults.env`

### Some optimization

- Load the `.env` variables into the session and the values stored in the session will only be loaded if a variable is not found
- Create session variable setter and getter to not access the globals all the time
- Create shutdown handler which will dump the `.env` settings in the session as the runtime ends
- Create spl_autoloader got classes using namespaces equal to the folder structure
- Added a `bootstrap.php` file that loads all the `.env` class and some helper functions and some constants
- Error reporting was managed by apache, now also set by the `.env` variables

### Some final things to test

= will `.default.env` be accessible by http / https on port 80?
