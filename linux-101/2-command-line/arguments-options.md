# Command Line Arguments and Options

- Some options can be specified by a single dash and a single character, other options require two dashes and a multi-character (verbose) description.
- Formats can also be combined, by following some rules:
  - one character options can be merged
  - multi-character formats cannot be combined

## ls command example

```bash
ls --help
	Usage: ls [OPTION]... [FILE]...
	List information about the FILEs (the current directory by default).
	Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.
	
	Mandatory arguments to long options are mandatory for short options too.
	  -a, --all                  do not ignore entries starting with .
	  -A, --almost-all           do not list implied . and ..
	[...]
	  -I, --ignore=PATTERN       do not list implied entries matching shell PATTERN
	  -k, --kibibytes            default to 1024-byte blocks for disk usage;
	                               used only with -s and per directory totals
	  -l                         use a long listing format
	  -L, --dereference          when showing file information for a symbolic
	                               link, show information for the file the link
	                               references rather than for the link itself
	  -m                         fill width with a comma separated list of entries
	  -n, --numeric-uid-gid      like -l, but list numeric user and group IDs
	[...]
```

- **`ls -al`** - Single character options:

```bash
ls -al
	total 100
	drwxr-x--- 16 user user 4096 ago 28 15:15 .
	drwxr-xr-x  3 root root 4096 lug 24 15:19 ..
	-rw-------  1 user user  829 ago  8 10:44 .bash_history
	-rw-r--r--  1 user user  220 lug 24 15:19 .bash_logout
	-rw-r--r--  1 user user 3771 lug 24 15:19 .bashrc
	drwx------ 12 user user 4096 ago 28 14:41 .cache
	drwx------ 12 user user 4096 ago 28 14:41 .config
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Desktop
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Documents
	drwxr-xr-x  3 user user 4096 lug 24 15:59 Downloads
	drwx------  2 user user 4096 ago 28 15:38 .gnupg
	-rw-------  1 user user   20 ago 28 15:15 .lesshst
	drwx------  3 user user 4096 lug 24 15:37 .local
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Music
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Pictures
	-rw-r--r--  1 user user  807 lug 24 15:19 .profile
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Public
	drwx------  4 user user 4096 lug 24 15:59 snap
	drwx------  2 user user 4096 lug 24 15:40 .ssh
	-rw-r--r--  1 user user    0 lug 24 15:37 .sudo_as_admin_successful
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Templates
	-rw-r-----  1 user user    5 ago 28 14:41 .vboxclient-clipboard.pid
	-rw-r-----  1 user user    5 ago 28 14:41 .vboxclient-display-svga-x11.pid
	-rw-r-----  1 user user    5 ago 28 14:41 .vboxclient-draganddrop.pid
	-rw-r-----  1 user user    5 ago 28 14:41 .vboxclient-seamless.pid
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Videos
```

- **`ls --all -l`** - Multi character options:

```bash
# Same output
ls --all -l
	total 100
	drwxr-x--- 16 user user 4096 ago 28 15:15 .
	drwxr-xr-x  3 root root 4096 lug 24 15:19 ..
	-rw-------  1 user user  829 ago  8 10:44 .bash_history
	-rw-r--r--  1 user user  220 lug 24 15:19 .bash_logout
	-rw-r--r--  1 user user 3771 lug 24 15:19 .bashrc
	drwx------ 12 user user 4096 ago 28 14:41 .cache
	drwx------ 12 user user 4096 ago 28 14:41 .config
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Desktop
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Documents
	drwxr-xr-x  3 user user 4096 lug 24 15:59 Downloads
	drwx------  2 user user 4096 ago 28 15:38 .gnupg
	-rw-------  1 user user   20 ago 28 15:15 .lesshst
	drwx------  3 user user 4096 lug 24 15:37 .local
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Music
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Pictures
	-rw-r--r--  1 user user  807 lug 24 15:19 .profile
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Public
	drwx------  4 user user 4096 lug 24 15:59 snap
	drwx------  2 user user 4096 lug 24 15:40 .ssh
	-rw-r--r--  1 user user    0 lug 24 15:37 .sudo_as_admin_successful
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Templates
	-rw-r-----  1 user user    5 ago 28 14:41 .vboxclient-clipboard.pid
	-rw-r-----  1 user user    5 ago 28 14:41 .vboxclient-display-svga-x11.pid
	-rw-r-----  1 user user    5 ago 28 14:41 .vboxclient-draganddrop.pid
	-rw-r-----  1 user user    5 ago 28 14:41 .vboxclient-seamless.pid
	drwxr-xr-x  2 user user 4096 lug 24 15:37 Videos
```

- **`ls -I`** - use ignore pattern option:

```bash
# Ignore all entries that starts with the letter D:

ls -I "D*"
	Music  Pictures  Public  snap  Templates  Videos

ls --ignore="D*"
	Music  Pictures  Public  snap  Templates  Videos
```

> 📌 Pay attention to option arguments when combining more single character options. It is better to use the long format option.

------

