Geolier
==============

Geolier is an utility to configure a backup server and create backup clients in a jail using JailKit

Quick Start
-----------

Imagine you have several servers to backup, each may or may not be owned by differents clients, you may
want to store backup remotely to improve redondancy, in this case, you need to create one access for each
servers, on the same server.

It means that if one of the server is compromised, all others backups on the server may be too.

So here is the trick, put each users in a jail, with a minimal set of commands, just enough to properly
handle received backups no matter how it is used (scp, sftp, piped ssh, remote tar, etc.)

Table of contents
-----------------

1. [Installation](#installation)
2. [Getting started](#getting-started)
3. [Usage](#usage)
4. [Contributing](#contributing)
5. [Requirements](#requirements)
6. [Authors](#authors)
7. [License](#license)

Description
-----------

Geolier is an utility to configure a backup server and create backup clients in a jail using JailKit

Installation
------------

Getting started
---------------

Usage
-----

```bash
usage: ./geolier <command> [-h] [-vvv] [-d] [<args>]

Currently supported commands are:
    create [<username>] Create a new backup user and its jail
    delete [<username>] Remove a backup user and its jail (including its data)
    install             Install jailkit
```

* `-v` set the level of verbosity (more `v`, more verbose)
* `-d` dry-run, do not actually run commands


Examples:

`geolier create`

```bash
root@b01:~# ./geolier create -v -d backup_user
-> useradd -m -N -g backup -s /usr/sbin/jk_chrootsh backup_user
-> mkdir /srv/jails/backup_user
-> chown root:root /srv/jails/backup_user
-> jk_init -v -j /srv/jails/backup_user basicshell editors extendedshell netutils ssh uidbasics jk_lsh scp custom
-> jk_jailuser -m -j /srv/jails/backup_user backup_user
-> sed -E -i s/(^backup_user:.*:).*$/\1\/bin\/bash/ /srv/jails/backup_user/etc/passwd
-> ssh key fingerprint: (de:cf:7c:1e:85:05:ea:ce:e4:c0:6c:b3:43:7f:78:1a)
-> mkdir /srv/jails/backup_user/home/backup_user/.ssh/
-> mv /tmp/tmp.4dwnfMMy0n /srv/jails/backup_user/home/backup_user/.ssh/authorized_keys
-> chown -R backup_user /srv/jails/backup_user/home/backup_user/.ssh
-> chmod 500 /srv/jails/backup_user/home/backup_user/.ssh/
-> chmod 600 /srv/jails/backup_user/home/backup_user/.ssh/authorized_keys
```


Contributing
------------

1. Take a look at the [list of issues](http://github.com/borisguery/geolier/issues).
2. Fork
3. Write a test (for either new feature or bug)
4. Make a PR

Requirements
------------

* bash >= 4
* common build tools (gcc, etc.), required to build JailKit

Authors
-------

Boris Guéry - guery.b@gmail.com - http://borisguery.com - [@borisguery](https://twitter.com/borisguery)

License
-------

`Geolier` is licensed under the MIT License - see the LICENSE file for details
