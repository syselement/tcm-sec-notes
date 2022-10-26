# File Transfer

- Use the command line shell to fransfer files.

## Commands

### scp

- **`scp`** - secure file/dir copy using encrypted SSH protocol

```bash
# Copy file from a local pc to a remote pc
scp file.txt 192.168.1.50:/home/user/
# scp Source_path Destination_path

# Copy a directory and its contents
scp -r files 192.168.1.50:/home/user/
# "files" is a directory

# Copy file from remote
scp 192.168.1.50:/home/user/remote-file.txt /home/user/

# Use a remote user
scp file.txt user2@192.168.1.50:/home/user2/
file.txt     100%    8     1.9KB/s   00:00
```

### rsync

- **`rsync`** - fast and versatile file-copying tool for synchronizing files and directories between two locations over a remote shell (or local)
  - it is widely used for backups and mirroring
  - it sends only the differences between the source files and the existing files in the destination.


```bash
# Copy file from local pc to remote pc
rsync -azvh file2.txt user@192.168.1.50:/tmp/
    user@192.168.1.50's password: 
    sending incremental file list
    file2.txt
    sent 119 bytes  received 35 bytes  44.00 bytes/sec
    total size is 7  speedup is 0.05
# a = archive mode - recursive copying into directories and preserve files user permissions and ownership
# v = verbose mode
# z = compress data during transfer (beneficial for large files)
# h = human readable format output

rsync -azvh --progress file2.txt user@192.168.1.50:/tmp/
    file2.txt     7 100%    0.00kB/s    0:00:00 (xfr#1, to-chk=0/1)
    sent 119 bytes  received 35 bytes  34.22 bytes/sec
    total size is 7  speedup is 0.05
# Check the total bytes sent

--dry-run # Test run with no changes made
```

------

