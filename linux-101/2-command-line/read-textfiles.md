# Looking at Text Files

## Commands

### more

- **`more`** - look at the content of a text file page by page, by only moving forward in a file. Press **`q`** to quit.

### less

- **`less`** - look at the content of a text file, with more options than the `more` command, like line by line, up and down navigation, moving, searching, jumping.
  - Search is possible by using the slash - **`/word_to_search`**
  - Press **`q`** to quit.

```bash
# Some "less" Commands and Actions
Down arrow, Enter, e, or j - Move forward one line.
Up arrow,y or k	- Move backward one line.
Space bar or f - Move Forward one page.
b - Move Backward one page.
/pattern - Search forward for matching patterns.
?pattern - Search backward for matching patterns.
n - Repeat previous search.
N - Repeat previous search in reverse direction.
g - Go to the first line in the file.
Ng - Go to the N-th line in the file.
G - Go to the last line in the file.
p - Go to the beginning of fthe ile.
Np - Go to N percent into file.
h - Display help.
q - Exit less.
```

### cat

- **`cat`** - Concatenate or link the content of different files to standard output.
  - Useful when combined with redirection of the output.


```bash
cat --help
	Usage: cat [OPTION]... [FILE]...
	Concatenate FILE(s) to standard output.
	With no FILE, or when FILE is -, read standard input.
	  -A, --show-all           equivalent to -vET
	  -b, --number-nonblank    number nonempty output lines, overrides -n
	  -e                       equivalent to -vE
	  -E, --show-ends          display $ at end of each line
	  -n, --number             number all output lines
	  -s, --squeeze-blank      suppress repeated empty output lines
	  -t                       equivalent to -vT
	  -T, --show-tabs          display TAB characters as ^I
	  -u                       (ignored)
	  -v, --show-nonprinting   use ^ and M- notation, except for LFD and TAB
	      --help     display this help and exit
	      --version  output version information and exit
	Examples:
	  cat f - g  Output f's contents, then standard input, then g's contents.
	  cat        Copy standard input to standard output.
	  
	  # Send the content of more files to a new file
	  cat file1.txt file2.txt file3.txt > combined.txt
	  cat combined.txt
```

- Display file contents:

```bash
cat /etc/os-release
	PRETTY_NAME="Ubuntu 22.04 LTS"
	NAME="Ubuntu"
	VERSION_ID="22.04"
	VERSION="22.04 LTS (Jammy Jellyfish)"
	VERSION_CODENAME=jammy
	ID=ubuntu
	ID_LIKE=debian
	HOME_URL="https://www.ubuntu.com/"
	SUPPORT_URL="https://help.ubuntu.com/"
	BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
	PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
	UBUNTU_CODENAME=jammy
```

------

