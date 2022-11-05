# Working with Users and Groups

- Based on a user's ID and the groups it is a part of, Linux manages access to resources.

## Commands

- Print information about users who are currently logged in.

### users, who, w, whoami

```bash
users
# Output:
	user

who
# Output:
	user     :0           2022-09-02 19:26 (:0)

who -a
	system boot  2022-11-03 21:44
	run-level 5  2022-11-03 21:44
user     + tty2         2022-11-03 21:45  old         1574 (tty2)
	pts/1        2022-11-05 10:44              6168 id=ts/1  term=0 exit=130
	pts/2        2022-11-05 10:30              6064 id=ts/2  term=0 exit=0
	pts/3        2022-11-05 10:30              6065 id=ts/3  term=0 exit=1

w
# Output info and processes about the logged on users:
	19:56:27 up  9:19,  1 user,  load average: 0,03, 0,01, 0,00
	USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
	user     :0       :0               02set22 ?xdm?   3:15   0.00s /usr/lib/gdm3/gdm-x-session --run-sc

whoami
# Output:
	user
```

## Users

- An account is based on a **user**, a **password**, a **home directory** and other information, that can be seen in some of the Linux O.S. configuration text files.

### /etc/passwd

```bash
cat /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    [...]
    gdm:x:125:130:Gnome Display Manager:/var/lib/gdm3:/bin/false
    user:x:1000:1000:user,,,:/home/user:/bin/bash
```

`user:x:1000:1000:user,,,:/home/user:/bin/bash`

- 1st column - **user name**
- 2nd column *used to be the hash* of the user's password, replaced by an **x**
  - `*` in the second column disable logins to the account
  - Password hashes were moved to the `/etc/shadow` file
- 3rd column - **user's UID** (User's **U**nique **Id**entifier)
- 4th column - user's **primary group ID** - **GID**
  - both UIDs are used for file permissions and process managing
- 5th column - additional text information, each info separated by a comma `,` - **GECOS**
- 6th column - **user's home directory**
- 7th (last) column - **user's default login shell**
  - `/usr/sbin/nologin` and `/bin/false` are used to prevent interactive shell logon with that user
- Normal user accounts start with **UID 1000**

> 📌 [/etc/passwd](https://linuxize.com/post/etc-passwd-file/) explained.
>
> 📌 [/etc/shadow](https://linuxize.com/post/etc-shadow-file/) explained.

```bash
ls -la /home
# List users home directories, owned by specific users and groups
```

## Groups

### /etc/group

```bash
# List all groups with:
cat /etc/group
# or
getent group
# includes LDAP users if present
	root:x:0:
    daemon:x:1:
    bin:x:2:
    sys:x:3:
    adm:x:4:syslog,user
    tty:x:5:sysl
    [...]
    sudo:x:27:user
    [...]
    gdm:x:130:
    lxd:x:131:user
    user:x:1000:
    sambashare:x:132:user
    systemd-coredump:x:999:
    mlocate:x:133:

groups
# List current user's groups
	user adm cdrom sudo dip plugdev lpadmin lxd sambashare
	
id user
# List UID, GID, secondary groups
	uid=1000(user) gid=1000(user) groups=1000(user),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),131(lxd),132(sambashare)
```

`user:x:1000:`

- 1st column - **group name**. Primary group of the user is the unique group associated only with that user.
- 2nd column - group password, not used, replaced by an **x**
- 3rd column - group's unique identifier - **GID**
- 4th last column - comma separated list of group members
- **adm** (administrator level privileges) and **sudo** (run commands as any user/group) groups are very important groups with high level privileges.

> 📌 [List groups](https://linuxize.com/post/how-to-list-groups-in-linux/) explained.

------

