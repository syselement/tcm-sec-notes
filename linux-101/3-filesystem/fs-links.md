# Hard and Soft Filesystem Links

- Linux offer shortcut function through links.
- A link is a reference or a pointer to a file or directory somewhere on the file system.
- There are two types of links: **hard** and **soft** (symbolic) links.

### ln

- **`ln`** - make links between files

```bash
ln --help
	Usage: ln [OPTION]... [-T] TARGET LINK_NAME
	  or:  ln [OPTION]... TARGET
	  or:  ln [OPTION]... TARGET... DIRECTORY
	  or:  ln [OPTION]... -t DIRECTORY TARGET...
	In the 1st form, create a link to TARGET with the name LINK_NAME.
	In the 2nd form, create a link to TARGET in the current directory.
	In the 3rd and 4th forms, create links to each TARGET in DIRECTORY.
	Create hard links by default, symbolic links with --symbolic.
	By default, each destination (name of new link) should not already exist.
	When creating hard links, each TARGET must exist.  Symbolic links
	can hold arbitrary text; if later resolved, a relative link is
	interpreted in relation to its parent directory.
```

- By default `ln` creates a Hard Link.

## Hard Link

- A **hard link** points to the physical location of the file on storage.
  - If the original file is moved or deleted a hard link continues to work. The file will be completely deleted when there are no hard links to the file.
  - Hard links are older and less frequently uses.
  - Cannot create directory hard link.


```bash
cat hello.txt 
	Hello

ln hello.txt hello-hard-link.txt

ls hello*
	hello-hard-link.txt  hello.txt

# After editing the original hello.txt file, the change is visible in the hello-hard-link.txt file
cat hello.txt
	Hello World!!!

cat hello-hard-link.txt
	Hello World!!!

# If original file is delete, the link file still exists with the same content
rm hello.txt 
cat hello-hard-link.txt
	Hello World!!!

# Recreate the original file
ln hello-hard-link.txt hello.txt
```

## Soft - Symbolic Link

- A **symbolic link** references the file or directory on the file system, not on storage.
  - If the resource is moved or deleted from the file system, the symbolic link will not work.
  - Always use **absolute paths** for soft links targets.
  - Soft links can cross file systems.
  - Directory soft links can be created.
  - There are many internal system symbolic links.


```bash
ln -s ./hello.txt hello-soft-link.txt

ls -l hello*
	-rw-rw-r-- 2 user user 15 ago 28 19:50 hello-hard-link.txt
	lrwxrwxrwx 1 user user 11 ago 28 19:56 hello-soft-link.txt -> ./hello.txt
	-rw-rw-r-- 2 user user 15 ago 28 19:50 hello.txt

# The soft link created references the original hello.txt file
cat hello-soft-link.txt 
	Hello World!!!

# If original file is moved, the soft link won't work
mv hello.txt hello-new.txt
cat hello-soft-link.txt
	cat: hello-soft-link.txt: No such file or directory
```

------

