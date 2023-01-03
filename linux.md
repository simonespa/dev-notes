# Linux terminal

- [Convert from PNG to JPEG](#convert-from-png-to-jpeg)
- [Listening to port](#listening-to-port)
- [Kill a service listening on a specific port](#kill-a-service-listening-on-a-specific-port)
- [Change to a nologin user](#change-to-a-nologin-user)
- [.bashrc VS .bash_profile](#bashrc-vs-bash-profile)
- [Redirection of stdout/stderr](#redirection-of-stdout-stderr)
- [Get the last command's exit code](#get-the-last-command-s-exit-code)
- [Get the target of a symlink](#get-the-target-of-a-symlink)
- [Multiline in bash](#multiline-in-bash)
  - [Execute multiple commands on the terminal](#execute-multiple-commands-on-the-terminal)
  - [Pass a multiline string to a variable](#pass-a-multiline-string-to-a-variable)
  - [Pass a multiline string to a file](#pass-a-multiline-string-to-a-file)
  - [Pass a multiline string to a command/pipe](#pass-a-multiline-string-to-a-command-pipe)
- [Linux filesystem](#linux-filesystem)
  - [/bin](#-bin)
  - [/sbin](#-sbin)
  - [/usr/bin](#-usr-bin)
  - [/user/sbin](#-user-sbin)
  - [/usr/local/bin](#-usr-local-bin)
  - [/usr/local/sbin](#-usr-local-sbin)
  - [Where should my own script live?](#where-should-my-own-script-live-)
- [Man](#man)
- [Users & Groups](#users---groups)
- [Other commands](#other-commands)
- [References](#references)
  - [Init Scripts](#init-scripts)
  - [Linux Permissions and Sticky bit](#linux-permissions-and-sticky-bit)
  - [SAR](#sar)

## Convert from PNG to JPEG

To convert one file only

```
convert <FILE>.png <FILE>.jpeg
```

For batch operations

```
for i in *.png ; do convert "$i" "${i%.*}.jpeg" ; done
```

## Listening to port

```
sudo lsof -nP | grep LISTEN
```

## Kill a service listening on a specific port

```
lsof -i:8080
kill $(lsof -t -i:8080)
```

## Change to a nologin user

```
sudo -u <username> bash
```

## .bashrc VS .bash_profile

The `.bash_profile` is executed for login shells, while `.bashrc` is executed for interactive non-login shells.

When you login (i.e. type username and password) via the console - either sitting at the machine or remotely via ssh - the `.bash_profile` is executed to configure your shell before the initial command prompt.

If you’ve already logged into your machine and open a new terminal window (xterm) then the `.bashrc` is executed before the window command prompt.

The `.bashrc` is also run when you start a new bash instance by typing /bin/bash in a terminal.

On the MacOS Terminal a login shell is run by default. This is a little different to most other systems, but you can configure that in the preferences.

## Redirection of stdout/stderr

- `>` file redirects stdout to file
- `1>` file redirects stdout to file
- `2>` file redirects stderr to file
- `&>` file redirects stdout and stderr to file

## Get the last command's exit code

```
echo $?
```

## Get the target of a symlink

```
readlink -f /usr/bin/python
```

## Multiline in bash

### Execute multiple commands on the terminal

```
bash <<EOF
...
EOF
```

### Pass a multiline string to a variable

```
sql=$(cat <<EOF
SELECT foo, bar FROM db
WHERE foo='baz'
EOF
)
```

### Pass a multiline string to a file

```
cat <<EOF > print.sh
#!/bin/bash
echo \$PWD
echo $PWD
EOF
```

### Pass a multiline string to a command/pipe

```
cat <<EOF | grep 'b' | tee b.txt | grep 'r'
foo
bar
baz
EOF
```

## Linux filesystem

Please refer to the Filesystem Hierarchy Standard (FHS) for Linux.

### /bin

For binaries usable before the /usr partition is mounted. This is used for trivial binaries used in the very early boot stage or ones that you need to have available in booting single-user mode. Think of binaries like cat, ls, etc.

The `/bin` and `/sbin` paths were intended for programs that needed to be on a small / partition before the larger /usr, etc. partitions were mounted. These days, it mostly serves as a standard location for key programs like /bin/sh, although the original intent may still be relevant for e.g. installations on small embedded devices.

### /sbin

Same as `/bin`, but for scripts with superuser (root) privileges required.

Unlike `/bin` though, it is for system management programs - not normally used by ordinary users - needed before `/usr` is mounted.

### /usr/bin

Same as above, but for general system-wide binaries. It is used for distribution-managed normal user programs.

There is a `/usr/sbin` with the same relationship to `/usr/bin` as `/sbin` has to `/bin`.

### /user/sbin

Same as above, but for scripts with superuser (root) privileges required.

### /usr/local/bin

is for normal user programs not managed by the distribution package manager, e.g. locally compiled packages. You should not install them into /usr/bin because future distribution upgrades may modify or delete them without warning.

### /usr/local/sbin

As you can probably guess at this point, is to `/usr/local/bin` as `/usr/sbin` is to `/usr/bin`. In addition, there is also `/opt` which is for monolithic non-distribution packages, although before they were properly integrated various distributions put Gnome and KDE there. Generally you should reserve it for large, poorly behaved third party packages such as Oracle.

### Where should my own script live?

Please use `/usr/local/bin` or `/usr/local/sbin` for system-wide available scripts. The local path means it's not managed by the system packages (this is an error for Debian/Ubuntu packages).

For user-scoped scripts, use `bin` in your home directory.

## Man

Ref http://superuser.com/questions/357048/how-do-you-switch-between-linux-manual-pages

```
Section     Description
1   General commands
2   System calls
3   C library functions
4   Special files (usually devices, those found in /dev) and drivers
5   File formats and conventions
6   Games and screensavers
7   Miscellanea
8   System administration commands and daemons
whatis <COMMAND>
man <N> <COMMAND>
```

## Users & Groups

When we run the `useradd` command in Linux terminal, it performs the following major things:

1. It edits `/etc/passwd`, `/etc/shadow`, `/etc/group` and `/etc/gshadow` files for the newly created user
2. Creates and populates a home directory for the new user
3. Sets permissions and ownerships to the home directory

```
useradd <USER_NAME>
usermod -aG <GROUP_NAME>,[<GROUP_NAME>] <USER_NAME>
```

## Other commands

- `logger`
- `dirname`
- `basename`
- `lsof`
- `df`
- `du`
- `mknod`
- `netstat`
- `grep`
- `awk`
- `cat`
- `tee`
- `tail`

## References

### Init Scripts

- https://blog.hazrulnizam.com/create-init-script-centos-6/

### Linux Permissions and Sticky bit

- https://h3g3m0n.wordpress.com/2007/04/17/linuxunix-permissions/
- http://linux-tipps.blogspot.co.uk/2008/07/directory-rights-in-linux.html
- http://linux-101.org/quick-ref/linux-file-permissions

### SAR

http://www.linuxjournal.com/content/sysadmins-toolbox-sar
http://www.thegeekstuff.com/2011/03/sar-examples/
https://www.thomas-krenn.com/en/wiki/Collect_and_report_Linux_System_Activity_Information_with_sar
