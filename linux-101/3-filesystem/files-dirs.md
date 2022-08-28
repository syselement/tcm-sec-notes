# Working with Files and Directories

## Commands

### touch

- **`touch`** - updates the access and last modification times of each file to the current time.
  - If the specified file does not exist, the touch command will create it.
  - It is used more to create empty files than to update the file timestamp.

```bash
ls -l notes.txt
	-rw-rw-r-- 1 user user 0 ago 28 18:36 notes.txt

touch notes.txt
ls -l notes.txt
	-rw-rw-r-- 1 user user 0 ago 28 18:38 notes.txt
# Timestamp is updated to the current time
```

### cp

- **`cp`** - copy the source file to the destination file

```bash
cp notes.txt notes-copy.txt
```

### mv

- **`mv`** - move file
  - used for renaming the file/directory in the same directory

```bash
mv notes.txt ./Desktop
```

### rm

- **`rm`** - remove files or directories
  - deletes the file forever (does not put it in the recycle bin)
  - use various options to minimize accidents

```bash
rm -i ./Desktop/notes.txt
rm: remove regular empty file './Desktop/notes.txt'? y
```

### mkdir

- **`mkdir`** - create directories

```bash
mkdir project
# Creates project directory
```

### rmdir

- **`rmdir`** - remove empty directory

```bash
rmdir project/
rmdir: failed to remove 'project/': Directory not empty

# Use "rm" to delete a non-empty directory, recursively removing all the directory contents without confirmation:
rm -rf project/
```

------

