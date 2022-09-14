# Archiving and Searching Files

## Archiving Commands

### zip

- **`zip`** - package and compress files into archives
  - zip files archive a number of files into a single file
  - zip compress the files so the archive takes less space

```bash
zip /tmp/backup-notes.zip words.txt words2.txt
ls -l /tmp/
	-rw-rw-r-- 1 user user  499 ago 28 20:11 backup-notes.zip
	
zip -r /tmp/backup-dirs.zip dir1 dir2
# -r option must be used to include the files in the sub-directories
```

### unzip

- **`unzip`** - list, test and extract compressed files in a zip archive

```bash
# List archive files without extracting anything.

unzip -l /tmp/backup-notes.zip
	Archive:  /tmp/backup-notes.zip
	  Length      Date    Time    Name
	---------  ---------- -----   ----
	      128  2022-08-28 19:27   words.txt
	       87  2022-08-28 19:35   words2.txt
	---------                     -------
	      215                     2 files
```

### tar

- **`tar`** - creates an archive file from multiple files and directories, without compression by default
  - supports a vast range of compression algorithms
  - **`.tar`** and **`.gz`** files are more commonly used in Linux than zip files.


```bash
tar -cvf archive.tar file?.txt dir?

# Create and compress the archive with gzip
tar -czf archive.tar.gz file1.txt file2.txt

# List archive contents
tar -tvf archive.tar
	-rw-rw-r-- user/user         6 2022-09-03 00:16 file1.txt
	-rw-rw-r-- user/user         7 2022-09-03 00:16 file2.txt
	-rw-rw-r-- user/user         8 2022-09-03 00:16 file3.txt
	drwxrwxr-x user/user         0 2022-09-03 00:11 dir1/
	-rw-rw-r-- user/user         6 2022-09-03 00:11 dir1/file1.txt
	drwxrwxr-x user/user         0 2022-09-03 00:11 dir2/
	-rw-rw-r-- user/user         7 2022-09-03 00:11 dir2/file2.txt
	drwxrwxr-x user/user         0 2022-09-03 00:11 dir3/
	-rw-rw-r-- user/user         8 2022-09-03 00:11 dir3/file3.txt

# Extract file from an archive in the current directory
tar -xvf archive.tar

# Help
tar --help
	[...]
	Compression options:
	  -a, --auto-compress        use archive suffix to determine the compression
	                             program
	  -I, --use-compress-program=PROG
	                             filter through PROG (must accept -d)
	  -j, --bzip2                filter the archive through bzip2
	  -J, --xz                   filter the archive through xz
	      --lzip                 filter the archive through lzip
	      --lzma                 filter the archive through xz
	      --lzop                 filter the archive through lzop
	      --no-auto-compress     do not use archive suffix to determine the
	                             compression program
	  -z, --gzip, --gunzip, --ungzip   filter the archive through gzip
	      --zstd                 filter the archive through zstd
	  -Z, --compress, --uncompress   filter the archive through compress
	[...]
```

### gzip

- **`gzip`** - compress or uncompress single files
  - is most often used to compress text files, .tar archives, and web pages.
  - by default, it keeps the original file timestamp, mode, ownership, and name in the compressed file.

```bash
gzip -k archive.tar
# -k to keep the original file

# Check the compressed file dimension:
	-rw-rw-r--  1 user user 10240 set  3 00:16 archive.tar
	-rw-rw-r--  1 user user   151 set  3 00:24 archive.tar.gz
	
# Decompress a file and keep the input file:
gzip -dk archive.tar.gz
gunzip archive.tar.gz

```

## Searching Commands

### find

- **`find`** - search for files in a directory hierarchy
  - a starting point for the search is necessary

```bash
find . -name 'file*.txt'
# . = current directory and subdirectories starting point
    ./file1.txt
    ./file2.txt
    ./dir1/file1.txt
    ./dir3/file3.txt
    ./file3.txt
    ./dir2/file2.txt

find . -iname 'file*.txt'
# -iname for case insensitive search
```

### locate

- **`locate`** - find files by name, using a database
  - it can be faster than `find` command

```bash
locate file*.txt
    /home/user/Documents/file1.txt
    /home/user/Documents/file2.txt
    /home/user/Documents/file3.txt
    /home/user/Documents/dir1/file1.txt
    /home/user/Documents/dir2/file2.txt
    /home/user/Documents/dir3/file3.txt
```

### which

- **`which`** - locate a command
  - it displays the full path of the command with its associated name

```bash
which ls
	/usr/bin/ls
```

### whereis

- **`whereis`** - locate the binary, source code and manual page for a command

```bash
whereis ls
	ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
```

------

