# Compressing and Archiving Files

## Commands

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
unzip -l /tmp/backup-notes.zip
	Archive:  /tmp/backup-notes.zip
	  Length      Date    Time    Name
	---------  ---------- -----   ----
	      128  2022-08-28 19:27   words.txt
	       87  2022-08-28 19:35   words2.txt
	---------                     -------
	      215                     2 files

# List archive files without extracting anything.
```

### tar

- **`tar`** - creates an archive file from multiple files and directories, without compression by default