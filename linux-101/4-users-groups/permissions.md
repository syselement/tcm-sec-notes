# File and Directory Permissions

- Linux makes use of a basic permissions system to control user access to files and directories.

## Commands

### **ls -la** 

- Long list current folder files and permissions

```bash
ls -la
    total 136
    drwxr-xr-x 24 user user  4096 set 12 18:48 .
    drwxr-xr-x  3 root root  4096 apr 13  2021 ..
    drwxrwxr-x  8 user user  4096 lug 25 19:36 .atom
    -rw-------  1 user user 13454 set 12 22:15 .bash_history
    -rw-r--r--  1 user user   220 apr 13  2021 .bash_logout
    -rw-r--r--  1 user user  3795 apr 28  2021 .bashrc
    drwx------ 18 user user  4096 apr 12 15:22 .cache
    drwxrwxr-x  2 user user  4096 apr 12 15:46 code
    drwx------ 24 user user  4096 set  6 21:09 .config
    drwxr-xr-x  2 user user  4096 lug 25 19:37 Desktop
    drwxr-xr-x  7 user user  4096 set 12 18:43 Documents
    [...]
```

- The first column displays the permissions, which consist of 10 fields divided into 4 blocks

**`d rwx r-x r-x`**

- 1st character - file type, **`-`** for regular file, **`d`** for directory, **`l`** for symbolic link.
- 2nd block - **owner** permissions (user in the 3rd `ls -la` output column)
- 3rd block - **group** permissions (group in the 4th `ls -la` output column)
- 4th block - others/**world** permissions
- **r** - read
- **w** - write
- **x** - execute
  - Execute permission for a directory determines if a user can execute a command in that directory

- **`root`** user can access any file on the filesystem!

> 📌 Understand [Linux file permissions](https://linuxize.com/post/understanding-linux-file-permissions/).

### chmod

- **`chmod`** - change file mode bits

```bash
chmod --help
    Usage: chmod [OPTION]... MODE[,MODE]... FILE...
      or:  chmod [OPTION]... OCTAL-MODE FILE...
      or:  chmod [OPTION]... --reference=RFILE FILE...
    Change the mode of each FILE to MODE.
    With --reference, change the mode of each FILE to that of RFILE.

      -c, --changes          like verbose but report only when a change is made
      -f, --silent, --quiet  suppress most error messages
      -v, --verbose          output a diagnostic for every file processed
          --no-preserve-root  do not treat '/' specially (the default)
          --preserve-root    fail to operate recursively on '/'
          --reference=RFILE  use RFILE's mode instead of MODE values
      -R, --recursive        change files and directories recursively
          --help     display this help and exit
          --version  output version information and exit
    Each MODE is of the form '[ugoa]*([-+=]([rwxXst]*|[ugo]))+|[-+=][0-7]+'.
```

- **Symbolic Mode**

```bash
ls -l file1.txt 
	-rw-rw-r-- 1 user user 6 set  3 00:16 file1.txt

# Change the file1.txt permissions, by removing group write permissions
chmod g-w file.txt
	-rw-r--r-- 1 user user 6 set  3 00:16 file1.txt

# Change permissions to allow only the owner to read the file
chmod a=,u=r file1.txt
	# No permissions for all users and read permissions for the owner
	-r-------- 1 user user 6 set  3 00:16 file1.txt
```

- **Numeric octal Mode**

| Octal number    | Permissions             | File listing |
| :-------------- | :---------------------- | :----------- |
| **7**   (4+2+1) | read, write and execute | rwx          |
| **6**   (4+2+0) | read and write          | rw-          |
| **5**   (4+0+1) | read and execute        | r-x          |
| **4**   (4+0+0) | read only               | r--          |
| **3**   (0+2+1) | write and execute       | -wx          |
| **2**   (0+2+0) | write onyl              | -w-          |
| **1**   (0+0+1) | execute only            | --x          |
| **0**   (0+0+0) | none                    | ---          |

```bash
stat -c "%a" file1.txt
	400

# Assign read/write permissions to owner and group, read permissions to everyone else
chmod 664 file1.txt
	-rw-rw-r-- 1 user user 6 set  3 00:16 file1.txt
```

> 📌 Check [advanced chmod usage](https://linuxize.com/post/chmod-command-in-linux/) and [NetTools calculator](https://nettools.club/chmod_calc) or [Chmod Calculator](https://chmod-calculator.com/).

### chown

- **`chown`** - change the owner and group of each file

```bash
chown --help
    Usage: chown [OPTION]... [OWNER][:[GROUP]] FILE...
      or:  chown [OPTION]... --reference=RFILE FILE...
    Change the owner and/or group of each FILE to OWNER and/or GROUP.
    With --reference, change the owner and group of each FILE to those of RFILE.

      -c, --changes          like verbose but report only when a change is made
      -f, --silent, --quiet  suppress most error messages
      -v, --verbose          output a diagnostic for every file processed
          --dereference      affect the referent of each symbolic link (this is
                             the default), rather than the symbolic link itself
      -h, --no-dereference   affect symbolic links instead of any referenced file
                             (useful only on systems that can change the
                             ownership of a symlink)
          --from=CURRENT_OWNER:CURRENT_GROUP
                             change the owner and/or group of each file only if
                             its current owner and/or group match those specified
                             here.  Either may be omitted, in which case a match
                             is not required for the omitted attribute
          --no-preserve-root  do not treat '/' specially (the default)
          --preserve-root    fail to operate recursively on '/'
          --reference=RFILE  use RFILE's owner and group rather than
                             specifying OWNER:GROUP values
      -R, --recursive        operate on files and directories recursively
    The following options modify how a hierarchy is traversed when the -R
    option is also specified.  If more than one is specified, only the final
    one takes effect.
      -H                     if a command line argument is a symbolic link
                             to a directory, traverse it
      -L                     traverse every symbolic link to a directory
                             encountered
      -P                     do not traverse any symbolic links (default)
          --help     display this help and exit
          --version  output version information and exit
    Owner is unchanged if missing.  Group is unchanged if missing, but changed
    to login group if implied by a ':' following a symbolic OWNER.
    OWNER and GROUP may be numeric as well as symbolic.
    Examples:
      chown root /u        Change the owner of /u to "root".
      chown root:staff /u  Likewise, but also change its group to "staff".
      chown -hR root /u    Change the owner of /u and subfiles to "root".

# Give file ownership to "root"
sudo chown root file1.txt
	-rw-rw-r-- 1 root user 6 set  3 00:16 file1.txt
# It can be used for group changing too with "root:root"
```

### chgrp

- **`chgrp`** - change the group of each file

```bash
sudo chgrp root file1.txt
	-rw-rw-r-- 1 root root 6 set  3 00:16 file1.txt
```

- These commands are useful when extracting files from an archive or making a directory available to all users (like a software in /opt).

------

