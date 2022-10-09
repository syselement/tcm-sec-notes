# Changing Users and Passwords

## Commands

- Some users are able to run commands as another user.

### sudo

- Usually the **`sudo`** command is used to execute a command using root privileges.
- Only elevate your privileges to *root* when needed.

```bash
sudo cat /etc/passwd
# Runs the cat command as root

sudo -u user2 cat /home/user2/test.txt
# Runs the cat command as "user2" user
```

### su

- **`su`** - change user

```bash
su user2

su --help
    Usage:
     su [options] [-] [<user> [<argument>...]]

    Change the effective user ID and group ID to that of <user>.
    A mere - implies -l.  If <user> is not given, root is assumed.

    Options:
     -m, -p, --preserve-environment      do not reset environment variables
     -w, --whitelist-environment <list>  don't reset specified variables

     -g, --group <group>             specify the primary group
     -G, --supp-group <group>        specify a supplemental group

     -, -l, --login                  make the shell a login shell
     -c, --command <command>         pass a single command to the shell with -c
     --session-command <command>     pass a single command to the shell with -c
                                       and do not create a new session
     -f, --fast                      pass -f to the shell (for csh or tcsh)
     -s, --shell <shell>             run <shell> if /etc/shells allows it
     -P, --pty                       create a new pseudo-terminal

     -h, --help                      display this help
     -V, --version                   display version
```

### passwd

- **`passwd`** - change user password

```bash
passwd
# Change current user's password

sudo passwd user2
# Change another user's password using an elevated user
```

------

