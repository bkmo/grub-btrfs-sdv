[![GitHub release](https://img.shields.io/github/release/Antynea/grub-btrfs.svg)](https://github.com/Antynea/grub-btrfs/releases)
![](https://img.shields.io/github/license/Antynea/grub-btrfs.svg)

## 💻 grub-btrfs 

##### BTC donation address: `1Lbvz244WA8xbpHek9W2Y12cakM6rDe5Rt`
- - -
### 🔎 Description:
grub-btrfs improves the grub bootloader by adding a btrfs snapshots sub-menu, allowing the user to boot into snapshots.

grub-btrfs supports manual snapshots as well as snapper, timeshift, and yabsnap created snapshots.

##### Warning: booting read-only snapshots can be tricky

If you wish to use read-only snapshots, `/var/log` or even `/var` must be on a separate subvolume.
Otherwise, make sure your snapshots are writable.
See [this ticket](https://github.com/Antynea/grub-btrfs/issues/92) for more info.


- - -
### ✨ What features does grub-btrfs have?
* Automatically lists snapshots existing on the btrfs root partition.
* Automatically detect if `/boot` is in a separate partition.
* Automatically detect kernel, initramfs and Intel/AMD microcode in `/boot` directory within snapshots.
* Automatically create corresponding menu entries in `grub.cfg`
* Automatically detect the type/tags/triggers and descriptions/comments of Snapper/Timeshift/Yabsnap snapshots.
* Automatically generate `grub.cfg` if you use the provided Systemd/ OpenRC service.

- - -
### 🛠️ Installation:
#### Arch Linux
The package is available in the extra repository [grub-btrfs](https://archlinux.org/packages/extra/any/grub-btrfs/)
```
pacman -S grub-btrfs
```

#### Gentoo
grub-btrfs is only available in the Gentoo User Repository (GURU) and not in the official Gentoo repository.  
If you have not activated the GURU yet, do so by running:
```
emerge -av app-eselect/eselect-repository 
eselect repository enable guru 
emaint sync -r guru 
```
If you are using Systemd on Gentoo, make sure the USE-Flag `systemd` is set. (Either globally in make.conf or in package.use for the package app-backup/grub-btrfs)
Without Systemd USE-Flag the OpenRC-daemon of grub-btrfs will be installed.

Emerge grub-btrfs via 
`emerge app-backup/grub-btrfs`


* Dependencies:
  * [btrfs-progs](https://archlinux.org/packages/core/x86_64/btrfs-progs/)
  * [grub](https://archlinux.org/packages/core/x86_64/grub/)
  * [bash >4](https://archlinux.org/packages/core/x86_64/bash/)

- - -
#### grub-btrfsd OpenRC instructions
To start the daemon run:
```bash
sudo rc-service grub-btrfsd start 
```

To activate it during system startup, run:
```bash
sudo rc-config add grub-btrfsd default 
```

##### 💼 Snapshots not in `/.snapshots` for OpenRC
By default the daemon is watching the directory `/.snapshots`. If the daemon should watch a different directory, it can be edited by passing different arguments to it.
Arguments are passed to grub-btrfsd via the file `/etc/conf.d/grub-btrfsd`. 
The variable `snapshots` defines the path the daemon will monitor for snapshots.

After editing, the file should look like this:
``` bash
# Copyright 2022 Pascal Jaeger
# Distributed under the terms of the GNU General Public License v3

## Where to locate the root snapshots
snapshots="/.snapshots" # Snapper in the root directory
#snapshots="/run/timeshift/backup/timeshift-btrfs/snapshots" # Timeshift < v22.06

## Optional arguments to run with the daemon
# Append options to this like this:
# optional_args="--syslog --timeshift-auto --verbose"
# Possible options are:
# -t, --timeshift-auto  Automatically detect Timeshifts snapshot directory for timeshift >= 22.06
# -o, --timeshift-old   Look for snapshots in directory of Timeshift <v22.06 (requires --timeshift-auto)
# -l, --log-file        Specify a logfile to write to
# -v, --verbose         Let the log of the daemon be more verbose
# -s, --syslog          Write to syslog
optional_args="--syslog"
```

After that, the daemon should be restarted with:
``` bash
sudo rc-service grub-btrfsd restart
```

- - -
### Troubleshooting
If you experience problems with grub-btrfs don't hesitate [to file an issue](https://github.com/Antynea/grub-btrfs/issues/new/choose).

#### What version of grub-btrfs am I running?
When requesting help or reporting bugs in grub-btrfs, please run:
``` bash
sudo /etc/grub.d/41_snapshots-btrfs --version
```
or 
``` bash
sudo /usr/bin/grub-btrfsd --help
```
to get the currently running version of grub-btrfs and include this information in your ticket.

#### Running grub-btrfsd in verbose mode
If you have problems with the daemon, you can run it with the `--verbose`-flag. To do so you can run:
``` bash
sudo /usr/bin/grub-btrfsd --verbose --timeshift-auto` (for timeshift)
# or 
sudo /usr/bin/grub-btrfsd /.snapshots --verbose` (for snapper/yabsnap)
```
Or pass `--verbose` to the daemon using the Systemd .service file or the OpenRC conf.d file respectively.

For additional information on the daemon and its arguments, run `grub-btrfsd -h` or `man grub-btrfsd`

- - -
### Development
Grub-btrfs uses a rudimentary system of automatic versioning to tell apart different commits. This is helpful when users report problems and it is not immediately clear what version they are using. 
We therefore have the following script in `.git/hooks/pre-commit`:

``` bash
#!/bin/sh

echo "Doing pre commit hook with version bump"
version="$(git describe --tags --abbrev=0)-$(git rev-parse --abbrev-ref HEAD)-$(date -u -Iseconds)"
echo "New version is ${version}"
sed -i "s/GRUB_BTRFS_VERSION=.*/GRUB_BTRFS_VERSION=${version}/" config
git add config
```

This automatically sets the version in the `config`-file to `[lasttag]-[branch-name]-[current-date-in-UTC]`.
In order to create a Tag we don't want to have this long version. In this case we set the version manually in `config` and commit with `git commit --no-verify`. This avoids running the hook. 

### Special thanks for assistance and contributions
* [Maxim Baz](https://github.com/maximbaz)
* [Schievel1](https://github.com/Antynea/grub-btrfs/discussions/173#discussioncomment-1438790)
* [All contributors](https://github.com/Antynea/grub-btrfs/graphs/contributors)
- - -
