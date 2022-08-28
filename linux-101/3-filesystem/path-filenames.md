# Paths and Filenames

## Spaces in filenames

> 📌 Use **`TAB`** for auto-completion.

```bash
ls
	'file name.txt'
# Single quotes are displayed to make sure "file name.txt" is one entry

cat file name.txt
	cat: file: No such file or directory
	cat: name.txt: No such file or directory
# Bash shell uses spaces between commands arguments
```

- Best pratice: do not use spaces in file names as word separators.
- There are two ways to tell bash the space is part of the name:
  - Escape the name with a **`\`** (treat the space as part of the argument).
  - Place the entire name in double quotes (do not interpret any special character inside the quotes).

```bash
cat file\ name.txt
	Hello Paths!
# TAB on keyboard can also be used for auto completion.

cat "file name.txt" 
	Hello Paths!
```

## File and Path Expansion

- Directories and files in a path are separated by a slash **`/`**.
- Everything in between separators is called a **segment**.
- The most used wildcard is the asterisk **`*`**.

```bash
ls file*.txt
# List the current directory text files that start with the string "file"

ls file?.txt
# List the text files that start with the string "file" and have another character after that.
# ? wildcard = any single character on the command line

ls **/*.txt
# Use ** to search accross multiple segments. ** matches zero or more characters accross multiple directories.
```

- **`[ ]`** indicate very specific character to match.

```bash
ls file[123].txt
ls file[1-3].txt
ls file[a-zA-Z].txt

# Only match files that start with the string "file" and another specific character after.
```

## Text Files Commands

### head

- **`head`** - by default print the first 10 lines of each file
  - Use **`-n`** option to specify the lines to display (or -NUMBER)

```bash
head words.txt 
	# One
	# Two
	# Three
	# Four
	# Five
	# Six
	# Seven
	# Eight
	# Nine
	# Ten
	
head -n 3 words.txt 
	# One
	# Two
	# Three
	
head -15 words.txt 
	# One
	# Two
	# Three
	# Four
	# Five
	# Six
	# Seven
	# Eight
	# Nine
	# Ten
	# 
	# Alfa
	# Bravo
	# Charlie
	# Delta
```

### tail

- **`tail`** - by default print the last 10 lines of each file
  - Use **`-n`** option to specify the lines to display (or +NUMBER)

```bash
tail words.txt 
	# Delta
	# Echo
	# Foxtrot
	# Golf
	# Hotel
	# India
	# Juliett
	# Kilo
	# Lima
	# Mike
	
tail -n 3 words.txt 
	# Kilo
	# Lima
	# Mike
	
tail -4 words.txt 
	# Juliett
	# Kilo
	# Lima
	# Mike
```

- **tail** command can be used to monitor the end of a file (a log) for changes.

```bash
tail --help
	-f, --follow[={name|descriptor}]
                           output appended data as the file grows;
                             an absent option argument means 'descriptor'
                             
tail -f /var/log/auth.log
```

### diff

- **`diff`** - compare files line by line and displays any differences

```bash
diff words.txt words2.txt 
	
	2c2
	< Two
	---
	> 
	4c4
	< Four
	---
	> 
	6c6
	< Six
	---
	> 
	8c8
	< Eight
	---
	> 
	13c13
	< Bravo
```

------

