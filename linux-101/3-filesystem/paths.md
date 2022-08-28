# Paths

- A path identifies the location of a file or directory.

## Absolute Paths

- An absolute path always starts **from the root directory** **`/`**.
- It is valid from anywhere on the file system, use it when certain of the location.
- Sometimes relative path can be longer than the absolute path, so an absolute path is prefered.

```bash
ls /var/log/syslog
# /var/log/syslog is an absolute path to the syslog file
```

## Relative Paths

- A relative path starts **from the current directory** to the resource.
- Is generally shorter than an absolute path, used for similar local directory structures, for projects.

```bash
cd /var
ls log/syslog
# log/syslog is a relative path

ls ./log/syslog
# Using "." it specifies the commands starts in the current working directory

ls ../home/user/Documents/
# ".." is the parrent of the current working directory
```

------

