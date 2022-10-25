# Command History & Substitution

## Command History

- To show previously executed commands use the `history` command

### history

```bash
history
  865  find / -name 'file1.txt' 2> errors.txt
  866  cat errors.txt 
  867  ll
  868  ls errors.txt 
  869  ll errors.txt 
  870  source .bashrc 
  871  ll errors.txt 
  872  ls -lah errors.txt
# It includes the executed commands and the history number associated to them

history | less

# To execute one of the previous commands use !HISTORY_NUMBER
!868

# To execute the last command use these shortcuts
!-1
!!

# Up & Down arrow keys can be used to scroll through history

# To execute the last "command" command - cat for example
!cat
```

- The configuration options of the history are located in the `.bashrc` file

```bash
cat ~/.bashrc
    # for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
    HISTSIZE=1000
    HISTFILESIZE=2000
```

## Command Substitution

- Redirecting doesn't always work.
- With **command substitution** a command can be replaced with its output before the entire command is executed by the shell.
- Backticks **``** or **$()** are used:

```bash
ls -l `cat file.txt`
ls -l $(cat file.txt)

# Example with a file list
cat file-list.txt 
    file1.txt
    file2.txt
    file3.txt
ls -l `cat file-list.txt`
    -rw-rw-r-- 1 root root 6 set  3 00:16 file1.txt
    -rw-rw-r-- 1 user user 7 set  3 00:16 file2.txt
    -rw-rw-r-- 1 user user 8 set  3 00:16 file3.txt
```

------

