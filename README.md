# ansible-role-jailkit

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-jailkit.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-jailkit)

RHEL/CentOS - chroot jail utilities

## Requirements

None

## Role Variables

Available variables are listed below, along with default values:

    jailkit_init:
      - name: basicshell
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
      - name: logbasics
        need_logsocket: True
        paths:
          - /etc/localtime
      - name: jk_lsh
        groups: [ 'root']
        includesections:
          - gidbasics
          - uidbasics
        paths:
          - /etc/jailkit/jk_lsh.ini
          - /sbin/jk_lsh
        users: [ 'root' ]
      - name: limitedshell
        includesections:
          - jk_lsh
      - name: netbasics
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
      - name: uidbasics
        paths:
          - /etc/nsswitch.conf
          - /etc/ld.so.conf
          - /lib/libnsl.so.1
          - /lib/libnss*.so.2
          - /lib64/libnsl.so.1
          - /lib64/libnss*.so.2

Additional variables that are not defined by default:

    jailkit_check:
      - path: /home/testchroot
        ignorepatheverywhere: []
        ignorepathoncompare: []
        ignoresetuidexecuteforuser: []
        ignoresetuidexecuteforgroup: []
        ignoresetuidexecuteforothers: []
        ignorewritableforgroup: []
        ignorewritableforothers: []

    jailkit_chrootsh:
      - name: tkimball
        env: [ 'DISPLAY' ]
        relax_home_group: True
        relax_home_group_permissions: True
        relax_home_other_permissions: True
      - name: users
        env:
          - TERM
          - PATH
        injail_shell: /bin/bash
        skip_injail_passwd_check: True
        type: group

    jailkit_lsh:
      - name: tkimball
        allow_word_expansion: True
        environment:
          - TERM=linux
        executables:
          - /usr/bin/rsync
        paths:
          - /usr/bin

    jailkit_socketd:
      - path: /home/testchroot/dev/log
        base: 512
        interval: 5.0
        peak: 2048

    jailkit_uchroot:
      - name: tkimball
        allowed_jails:
          - /srv/tkimball
        skip_injail_passwd_check: True
      - name: users
        allowed_jails:
          - /srv/users
        skip_injail_passwd_check: True
        type: group

    jailkit_update:
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
