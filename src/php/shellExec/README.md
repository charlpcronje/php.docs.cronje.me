---
title: Run Shell Exec as sudo | CRONje.ME
label: Run Shell Exec as sudo
order: 19
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
tags: [dev,tools,js,frontend,backend,devtools,php,laravel]
---
## Solution using a binary wrapper (with suid bit)

1. Create a script (preferrably .sh) that contains what you want to be ran as root.

```sh
# cat > php_shell.sh <<CONTENT
  #!/bin/sh
  /sbin/service sshd restart
CONTENT
```

2. This file should be owned by root, and since it will later run with root permissions make sure that only root has permission to write to the file.

```sh
# chown root php_shell.sh
# chmod u=rwx,go=xr php_shell.sh
```

3. To run the script as root no matter what user that executes it, we will need a binary wrapper. Create one that will execute our php_shell.sh.

```sh
# cat > wrapper.c <<CONTENT
  #include <stdlib.h>
  #include <sys/types.h>
  #include <unistd.h>

  int
  main (int argc, char *argv[])
  {
     setuid (0);

     /* WARNING: Only use an absolute path to the script to execute,
      *          a malicious user might fool the binary and execute
      *          arbitary commands if not.
      * */

     system ("/bin/sh /path/to/php_shell.sh");

     return 0;
   }
CONTENT
```

4. Compile and set proper permissions, including the suid bit (saying that it should run with root privileges):

```sh
# gcc wrapper.c -o php_root
# chown root php_root
# chmod u=rwx,go=xr,+s php_root
```

- `php_root` will now run with root permissions, and execute the commands specified in `php_shell.sh`.
- If you don't need to the option to easily change what commands that will be executed I'd recommend you to write the commands directly in `wrapper.c` under step 4. Then you don't need to have a binary executing a external script executing the commands in question.
- In `wrapper.c`, use system ("your shell command here"); to specify what commands you'd like to execute.