# Scheduling Processes

- All the processes are started when systems boots or when a command is run.
- Processes/commands can be scheduled to be executed at a specific time on a specific date by using the **`cron`** daemon.

## Commands

### crontab

- **`crontab`** - maintain crontab files for individual users
  - A **crontab** file contains instructions for the **[cron](https://crontab.guru/cron.8.html)** daemon
  - `crond` - daemon to execute scheduled commands
  - Each user can define their own crontab and the commands are executed under the user who owns that particular crontab

```bash
# Systems level crontabs
less /etc/crontab
    # /etc/crontab: system-wide crontab
    # Unlike any other crontab you don't have to run the `crontab'
    # command to install the new version when you edit this file
    # and files in /etc/cron.d. These files also have username fields,
    # that none of the other crontabs do.

    SHELL=/bin/sh
    # You can also override PATH, but by default, newer versions inherit it from the environment
    #PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

    # Example of job definition:
    # .---------------- minute (0 - 59)
    # |  .------------- hour (0 - 23)
    # |  |  .---------- day of month (1 - 31)
    # |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
    # |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
    # |  |  |  |  |
    # *  *  *  *  * user-name command to be executed
    17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
    25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
    47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
    52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
    #

# anacron runs processes on a set schedule (hourly, daily, weekly, monthly)

ls -lah /etc/cron.daily/
    total 56K
    drwxr-xr-x   2 root root 4,0K ott  5 09:09 .
    drwxr-xr-x 135 root root  12K nov  3 21:47 ..
    -rwxr-xr-x   1 root root  311 lug 16  2019 0anacron
    -rwxr-xr-x   1 root root  376 dic  4  2019 apport
    -rwxr-xr-x   1 root root 1,5K lug 28 09:26 apt-compat
    -rwxr-xr-x   1 root root  355 dic 29  2017 bsdmainutils.dpkg-remove
    -rwxr-xr-x   1 root root  384 nov 19  2019 cracklib-runtime
    -rwxr-xr-x   1 root root  123 dic  5  2021 dpkg
    -rwxr-xr-x   1 root root  377 gen 21  2019 logrotate
    -rwxr-xr-x   1 root root 1,3K mar 17  2022 man-db
    -rw-r--r--   1 root root  102 feb 13  2020 .placeholder
    -rwxr-xr-x   1 root root  652 dic  7  2020 plocate
# Every script is own by root, since they are associated to system-wide processes
```

```bash
crontab -e
# Edit user's crontab in a text editor (chose prefered text editor at first run)

# Expression Syntax:
m h  dom mon dow   command
    # m = minutes (0-59)
    # h = hours (0-23)
    # dom = day of the month (1-31)
    # mon = month (1-12)
    # dow = day of the week (0-6, sunday=0, saturday=6)
    # command to run according to the schedule

crontab -l
# List user's crontab

crontab -r
# Remove the current user's crontab

# List all user's crontabs
sudo su
for user in $(cut -f1 -d: /etc/passwd); do echo $user; crontab -u $user -l; done
```

> 📌 Check the [**crontab.guru**](https://crontab.guru/) editor for cron schedule expressions.

## Boot Process

- **`/etc/init.d`** - contains start/stop scripts for various services, while booting and shutting down the system
  - **init** (short for initialization) is the program that spawns all other processes. It runs as a daemon and typically has PID 1.
  - The applications that are started by init are located in the **`/etc/rc.d`** folder. Within this directory there is a separate folder for each run level, eg *rc0.d*, *rc1.d*, and so on.

```bash
ls -lah /etc/init.d/
    total 184K
    drwxr-xr-x   2 root root 4,0K ott 28 22:52 .
    drwxr-xr-x 135 root root  12K nov  3 21:47 ..
    -rwxr-xr-x   1 root root 2,3K nov 28  2019 acpid
    -rwxr-xr-x   1 root root 5,5K nov  5  2019 alsa-utils
    -rwxr-xr-x   1 root root 2,1K lug 16  2019 anacron
    -rwxr-xr-x   1 root root 3,7K apr  1  2020 apparmor
    -rwxr-xr-x   1 root root 2,9K mag 10 15:23 apport
    -rwxr-xr-x   1 root root 2,4K ago 21  2018 avahi-daemon
    -rwxr-xr-x   1 root root 2,9K feb 26  2020 bluetooth
    -rwxr-xr-x   1 root root 1,3K mar 27  2020 console-setup.sh
    -rwxr-xr-x   1 root root 3,0K mar 18  2021 cron
    -rwxr-xr-x   1 root root 2,8K feb 17  2020 cups
    -rwxr-xr-x   1 root root 2,0K apr 10  2020 cups-browsed
    -rwxr-xr-x   1 root root 3,1K set 30  2019 dbus
    -rwxr-xr-x   1 root root 3,0K apr 21  2022 gdm3
    -rwxr-xr-x   1 root root  985 gen 13  2021 grub-common
    -rwxr-xr-x   1 root root 1,8K feb 21  2022 hwclock.sh
    -rwxr-xr-x   1 root root 2,6K dic 13  2019 irqbalance
    -rwxr-xr-x   1 root root 3,1K mag 19  2017 kerneloops
    -rwxr-xr-x   1 root root 1,5K nov 27  2019 keyboard-setup.sh
    -rwxr-xr-x   1 root root 2,0K feb 19  2020 kmod
    -rwxr-xr-x   1 root root  695 gen 28  2020 lvm2
    -rwxr-xr-x   1 root root  586 gen 28  2020 lvm2-lvmpolld
    -rwxr-xr-x   1 root root 1,9K set 29  2020 open-vm-tools
    -rwxr-xr-x   1 root root 9,0K dic  3  2018 openvpn
    -rwxr-xr-x   1 root root 1,4K feb 23  2022 plymouth
    -rwxr-xr-x   1 root root  760 feb 23  2022 plymouth-log
    -rwxr-xr-x   1 root root  959 feb 25  2022 procps
    -rwxr-xr-x   1 root root  469 ott 15  2020 pulseaudio-enable-autospawn
    -rwxr-xr-x   1 root root 4,4K ott 15  2019 rsync
    -rwxr-xr-x   1 root root 2,2K feb 11  2020 saned
    -rwxr-xr-x   1 root root 2,0K gen 12  2020 speech-dispatcher
    -rwxr-xr-x   1 root root 2,5K set 27  2019 spice-vdagent
    -rwxr-xr-x   1 root root 6,8K giu 27 20:28 udev
    -rwxr-xr-x   1 root root 2,1K gen 21  2020 ufw
    -rwxr-xr-x   1 root root 1,4K lug 21  2020 unattended-upgrades
    -rwxr-xr-x   1 root root 1,3K lug 21  2020 uuidd
    -rwxr-xr-x   1 root root  485 ago 11  2018 whoopsie
    -rwxr-xr-x   1 root root 2,7K ott 19  2021 x11-common

ls -d /etc/rc*
    /etc/rc0.d  /etc/rc1.d  /etc/rc2.d  /etc/rc3.d  /etc/rc4.d  /etc/rc5.d  /etc/rc6.d  /etc/rcS.d

man runlevel
# Mapping between runlevels and systemd targets
       ┌─────────┬───────────────────┐
       │Runlevel │ Target            │
       ├─────────┼───────────────────┤
       │0        │ poweroff.target   │
       ├─────────┼───────────────────┤
       │1        │ rescue.target     │
       ├─────────┼───────────────────┤
       │2, 3, 4  │ multi-user.target │
       ├─────────┼───────────────────┤
       │5        │ graphical.target  │
       ├─────────┼───────────────────┤
       │6        │ reboot.target     │
       └─────────┴───────────────────┘

ls -lah /etc/rc5.d/
    total 16K
    drwxr-xr-x   2 root root 4,0K set 12 23:06 .
    drwxr-xr-x 135 root root  12K nov  3 21:47 ..
    lrwxrwxrwx   1 root root   27 apr 13  2021 K01speech-dispatcher -> ../init.d/speech-dispatcher
    lrwxrwxrwx   1 root root   15 apr 13  2021 S01acpid -> ../init.d/acpid
    lrwxrwxrwx   1 root root   17 apr 13  2021 S01anacron -> ../init.d/anacron
    lrwxrwxrwx   1 root root   16 apr 13  2021 S01apport -> ../init.d/apport
    lrwxrwxrwx   1 root root   22 apr 13  2021 S01avahi-daemon -> ../init.d/avahi-daemon
    lrwxrwxrwx   1 root root   19 apr 13  2021 S01bluetooth -> ../init.d/bluetooth
    lrwxrwxrwx   1 root root   26 apr 13  2021 S01console-setup.sh -> ../init.d/console-setup.sh
    lrwxrwxrwx   1 root root   14 apr 13  2021 S01cron -> ../init.d/cron
	[...]
    lrwxrwxrwx   1 root root   29 apr 13  2021 S01unattended-upgrades -> ../init.d/unattended-upgrades
    lrwxrwxrwx   1 root root   15 apr 13  2021 S01uuidd -> ../init.d/uuidd
    lrwxrwxrwx   1 root root   18 apr 13  2021 S01whoopsie -> ../init.d/whoopsie
```

------

