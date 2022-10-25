# Input/Output Redirection

> 📌 [Bash Redirection](https://linuxize.com/post/bash-redirect-stderr-stdout/)

- In Bash (or other Linux shells), when a program is executed, it uses three standard Input/Output streams, each one represented by a numeric file descriptor:
  - **0** - `stdin` : the standard input stream (printed on the screen by default)
  - **1** - `stdout` : the standard output stream
  - **2** - `stderr` : the standard error stream (printed on the screen by default)
- All three streams can be redirected.

```bash
sudo ls /root 
	[sudo] password for user: # This is the std input
	snap	# This is the std output

ls /root
	ls: cannot open directory '/root': Permission denied # This is the std error
```

## Redirection symbols

- The **`>`** symbol is used for redirect stdout to a file.
  - If the redirection points to a file that already exists, this file will be **overwritten**!

```bash
ls /etc/ > etc-contents.txt
# The content of the file is the same as the normal content of the command
```

- The **`>>`** symbol is used to append stdout to a file.

```bash
ls /tmp/ >> etc-contents.txt
```

- The **`<`** symbol is used for redirect stdin input to a command.

```bash
head < /etc/passwd
# The shell took the content of the passwd file and sent it to the head utility
```

- Redirect standard error to a file.
  - To suppress the error messagges from being displayed on the screen, redirect stderr to `/dev/null`.

```bash
find / -name 'file1.txt' 2> errors.txt
    /home/user/Documents/file1.txt
    /home/user/Documents/dir1/file1.txt

ls -lah errors.txt 
	-rw-rw-r-- 1 user user 89K ott 10 21:07 errors.txt

# Redirect to /dev/null
find / -name 'file1.txt' 2> /dev/null

# Redirect both stdout and stderr to the same file
find / -name 'file1.txt' &> all.txt

# Redirect stdout to all.txt and stderr to the same location as stdout
find / -name 'file1.txt' > all.txt 2>&1
```

## Pipes

- **`|`** - Pipes can connect the **stdout** of one command to the **stdin** of another command.

```bash
ls -l /etc/ | less
# Send the output of the ls command as input to the less command

ls -l /etc/ | head -n 20 | tail -n 5

find / -name 'file1.txt' | less

find / -name 'file1.txt' |& less
# Send stdout and stderr to less command
```

------

