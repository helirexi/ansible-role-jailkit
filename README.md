# ansible-role-jailkit

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-jailkit.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-jailkit)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-jailkit-blue.svg?style=flat)](https://galaxy.ansible.com/linuxhq/jailkit)
[![License](https://img.shields.io/badge/license-GPLv3-brightgreen.svg?style=flat)](COPYING)

RHEL/CentOS - chroot jail utilities

## Requirements

This role requires that you have the lux repository installed.

 * https://galaxy.ansible.com/linuxhq/lux/

## Role Variables

Available variables are listed below, along with default values:

    jk_check: []
    jk_chrootsh: []
    jk_create: []
    jk_init: []
    jk_lsh: []
    jk_socketd: []
    jk_uchroot: []
    jk_update: []

Example syntax for the listed variables below.

    jk_check:
      - path: /home/testchroot
        settings:
          ignorepatheverywhere: []
          ignorepathoncompare:
            - /home/testchroot/home
            - /home/testchroot/etc
          ignoresetuidexecuteforuser: /home/testchroot/usr/bin/smbmnt
          ignoresetuidexecuteforgroup: /home/testchroot/usr/bin/smbmnt
          ignoresetuidexecuteforothers: []
          ignorewritableforgroup: /home/testchroot/home
          ignorewritableforothers: /home/testchroot/home/tmp

    jk_chrootsh:
      - name: DEFAULT
        group: false
        settings:
          relax_home_group: 1
      - name: bill
        group: false
        settings:
          env:
            - DISPLAY
          relax_home_group: 1
          relax_home_group_permissions: 1
          relax_home_other_permissions: 1
      - name: jail
        group: true
        settings:
          env:
            - PATH
            - TERM

    jk_create:
      - path: /path/to/chroot
        group: root
        owner: root
        mode: 0755
        profile: netbasics

    jk_init:
      - name: netbasics
        settings:
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

    jk_lsh:
      - name: DEFAULT
        group: false
        settings:
          allow_word_expansion: 1
          executables:
            - /usr/bin/scp
            - /usr/lib/sftp-server
            - /usr/bin/rsync
          paths:
            - /usr/bin
            - /usr/lib
      - name: test
        group: false
        settings:
          allow_word_expansion: 0
          executables:
            - /usr/bin/scp
            - /usr/lib/sftp-server
          paths:
            - /usr/bin
            - /usr/lib
          umask: 002
      - name: test
        group: true
        settings:
          allow_word_expansion: 1
          environment:
            - TERM=linux
            - FOO=bar
          executables: /usr/bin/rsync
          paths: /usr/bin

    jk_socketd:
      - path: /home/testchroot/dev/log
        settings:
          base: 512
          interval: 5
          peak: 2048

    jk_uchroot:
      - name: john
        group: false
        settings:
          allowed_jails:
            - /srv/johnjail
            - /srv/commonjail
      - name: users
        group: true
        settings:
          allowed_jails:
            - /srv/commonjail
          skip_injail_passwd_check: 1

    jk_update:
      - path: /home/otherjail
        settings:
          skips:
            - /usr/share/firefox
            - /usr/bin/firefox
            - /usr/lib/firefox
      - path: /home/testchroot
        settings:
          directories:
            - /bin
            - /lib
          hardlinks: 1
          skips:
            - /usr/bin/firefox
            - /usr/lib/firefox
            - /usr/share/firefox

## Dependencies

None

## Example Playbook

    - hosts: servers
      roles:
        - role: linuxhq.jailkit
          jk_create:
            - path: /var/chroot/ngircd
              group: root
              owner: root
              mode: '0755'
              profile: netbasics
          jk_init:
            - name: netbasics
              settings:
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

## License

Copyright (C) 2018 Taylor Kimball <tkimball@linuxhq.org>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.
