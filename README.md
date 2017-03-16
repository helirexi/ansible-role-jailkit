# ansible-role-jailkit

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-jailkit.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-jailkit)

RHEL/CentOS - chroot jail utilities

## Requirements

None

## Role Variables

Available variables are listed below, along with default values:

    jk_init:
      basicshell:
        groups: [ 'root' ]
        includesections:
          - uidbasics
        paths:
          - /bin/bash
          - /bin/cat
          - /bin/chmod
          - /bin/cp
          - /bin/cpio
          - /bin/date
          - /bin/dd
          - /bin/echo
          - /bin/egrep
          - /bin/false
          - /bin/fgrep
          - /bin/grep
          - /bin/gunzip
          - /bin/gzip
          - /bin/ln
          - /bin/ls
          - /bin/mkdir
          - /bin/mktemp
          - /bin/more
          - /bin/mv
          - /bin/pwd
          - /bin/rm
          - /bin/rmdir
          - /bin/sed
          - /bin/sh
          - /bin/sleep
          - /bin/sync
          - /bin/tar
          - /bin/touch
          - /bin/true
          - /bin/zcat
          - /etc/motd
          - /etc/issue
          - /etc/bashrc
          - /etc/profile
        users: [ 'root' ]
      logbasics:
        need_logsocket: True
        paths:
          - /etc/localtime
      jk_lsh:
        groups: [ 'root']
        includesections:
          - gidbasics
          - uidbasics
        paths:
          - /etc/jailkit/jk_lsh.ini
          - /sbin/jk_lsh
        users: [ 'root' ]
      limitedshell:
        includesections:
          - jk_lsh
      netbasics:
        paths:
          - /lib/libnss_dns.so.2
          - /lib64/libnss_dns.so.2
          - /etc/host.conf
          - /etc/hosts
          - /etc/protocols
          - /etc/resolv.conf
          - /etc/services
          - /lib/libnss_dns.so.2
          - /lib64/libnss_dns.so.2
      uidbasics:
        paths:
          - /etc/nsswitch.conf
          - /etc/ld.so.conf
          - /lib/libnsl.so.1
          - /lib/libnss*.so.2
          - /lib64/libnsl.so.1
          - /lib64/libnss*.so.2

Additional variables that are not defined by default:

    jk_check:
      - path: /home/testchroot
        ignorepatheverywhere: []
        ignorepathoncompare: []
        ignoresetuidexecuteforuser: []
        ignoresetuidexecuteforgroup: []
        ignoresetuidexecuteforothers: []
        ignorewritableforgroup: []
        ignorewritableforothers: []

    jk_chrootsh:
      tkimball:
        env: [ 'DISPLAY' ]
        group: False
        relax_home_group: True
        relax_home_group_permissions: True
        relax_home_other_permissions: True
      users:
        env:
          - TERM
          - PATH
        group: True
        injail_shell: /bin/bash
        skip_injail_passwd_check: True

    jk_lsh:
      tkimball:
        allow_word_expansion: True
        environment:
          - TERM=linux
        executables:
          - /usr/bin/rsync
        group: False
        paths:
          - /usr/bin

    jk_socketd:
      - path: /home/testchroot/dev/log
        base: 512
        interval: 5.0
        peak: 2048

    jk_uchroot:
      tkimball:
        allowed_jails:
          - /srv/tkimball
        group: False
        skip_injail_passwd_check: True
      users:
        allowed_jails:
          - /srv/users
        group: True
        skip_injail_passwd_check: True

    jk_update:
      - path: /home/testchroot
        directories:
          - /bin
          - /lib
        hardlinks: True
        skips:
          - /usr/bin/firefox
          - /usr/lib/firefox
          - /usr/share/firefox

## Dependencies

 * https://galaxy.ansible.com/linuxhq/lux/

## Example Playbook

    - hosts: servers
      roles:
        - role: linuxhq.jailkit

## License

BSD

## Author Information

This role was created by [Taylor Kimball](http://www.linuxhq.org).
