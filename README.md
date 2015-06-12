<!--

---
lang: american
---
-->

[![Build Status](https://travis-ci.org/cw-ansible/cw.mount-extra.svg?branch=master)](https://travis-ci.org/cw-ansible/cw.mount-extra)
[![Tweet this](http://img.shields.io/badge/Tweet-it00aced.svg)](https://twitter.com/intent/tweet?tw_p=tweetbutton&via=renard_0&url=https%3A%2F%2Fgithub.com%2Fcw-ansible%2Fcw.mount-extra&text=Mount%20extra%20filesystems%20with%20%23ansible.)
[![Follow me on twitter](http://img.shields.io/badge/Twitter-Follow-00aced.svg)](https://twitter.com/intent/follow?region=follow_link&screen_name=renard_0&tw_p=followbutton)


# mount-extra

This roles configures the `/etc/mount-extra.cfg` file on
[Debian](http://debian.org) and [Ubuntu](http://ubuntu.com) like
distributions.

## Motivations

If you are working on golden images for your servers, you may not want to
mount all file systems on the master image but only in production. For
example you have a `/data` directory you want on production but not on the
master image. You can either deploy a specific `/etc/fstab` on each server
or you can use `mount-extra` (which does not fail if filesystem is not
available).

This role was written to cover the lack of support of `/etc/fstab.d/*` in
many distributions.

## Usage

Include the `cw.mount-extra` module to your playbook.

## Configuration

you have to configure the `mount_extra_fstab` variable as list of
dictionaries. Each item must have keys suitable for ansible the `mount`
module.

Handled options are:
 - `name`
 - `fstab`
 - `src`
 - `fstype`
 - `opts`
 - `state`

## How does it works?

The `/etc/mount-extra.cfg` file is parsed. When specific partition is found,
the target directory is created if not it does not exist. Then the partition
is mounted to its target.

An extra entry in `/etc/fstab` is created for convenience.


## Examples

### A simple `DATA` partition

    mount_extra_fstab:
      - name: /mnt
        src: 'label=DATA'
        fstype: xfs
        opts: rw,noatime,nobarrier,logbufs=8,logbsize=256k,inode64

Mount partition label `DATA` to `/mnt`

### Mount one partition by mysql instance and one `mysql_backup`

    mount_extra_fstab:
      - name: '/srv/mysql/\1'
        src: 'id=.*(db_\d{2})$'
        fstype: xfs
        opts: rw,noatime,nobootwait
      - name: /var/backups/mysql
        src: 'label=mysql_backup'
        fstype: xfs
        opts: rw,noatime,nobootwait


Each partition having a partition id ending by `db_XX` is mounted on
`/srv/mysql/db_XX`. The `mysql_backup` label is mounted on
`/var/backups/mysql`.




## Copyright

Author: Sébastien Gross `<seb•ɑƬ•chezwam•ɖɵʈ•org>` [@renard_0](https://twitter.com/renard_0)

License: WTFPL, grab your copy here: http://www.wtfpl.net
