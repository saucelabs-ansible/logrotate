# logrotate

[![Build Status](https://travis-ci.org/saucelabs-ansible/logrotate.svg?branch=master)](https://travis-ci.org/saucelabs-ansible/logrotate)

Installs logrotate and provides an easy way to setup additional logrotate scripts by
specifying a list of directives.

## Requirements

None

## Tests

| Family | Distribution | Version | Test Status |
|:-:|:-:|:-:|:-:|
| Debian | Ubuntu  | Precise | [![x86](http://img.shields.io/badge/x86-passed-006400.svg?style=flat)](#) [![x86_64](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#)  |
| Debian | Ubuntu  | Trusty  | [![x86](http://img.shields.io/badge/x86-passed-006400.svg?style=flat)](#) [![x86_64](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) |
| Debian | Ubuntu  | Vivid   | [![x86](http://img.shields.io/badge/x86-passed-006400.svg?style=flat)](#) [![x86_64](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) |
| Debian | Ubuntu  | Wily   | [![x86](http://img.shields.io/badge/x86-passed-006400.svg?style=flat)](#) [![x86_64](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) |


## Role Variables

* **logrotate_version**: the logrotate version to be installed (default: *).
* **logrotate_scripts**:
  list of logrotate scripts and the directives to use for the rotation,
  each list element should have the following keys:
    * name: name of the script that goes into /etc/logrotate.d/
    * path: list of paths which need log rotation
    * options: list of directives for logrotate, view the logrotate man page for specifics
    * scripts: dictionary of scripts for logrotate (see Example below)

```
logrotate_scripts:
  - name: rails
    path:
      - /srv/current/log/*.log
    options:
      - weekly
      - size 25M
      - missingok
      - compress
      - delaycompress
      - copytruncate
```

## Dependencies

None

## Example Playbook

Setting up logrotate for additional Nginx logs, with postrotate script.

```
- name: some play
  hosts: some-group
  gather_facts: yes

  roles:
    - role: logrotate
      logrotate_scripts:
        - name: nginx
          path:
            - /var/log/nginx/*.log
          options:
            - weekly
            - size 25M
            - rotate 7
            - missingok
            - compress
            - delaycompress
            - copytruncate
          scripts:
            postrotate: |
              [test -s /run/nginx.pid ] && kill -USR1 `cat /run/nginx.pid`
              # ability to add more lines to this section
      tags: logrotate
```

## License

[BSD](https://raw.githubusercontent.com/saucelabs-ansible/logrotate/master/LICENSE)

## Author Information

* [nickhammond](https://github.com/nickhammond) | [www](http://www.nickhammond.com) | [twitter](https://twitter.com/nickhammond)
* [bigjust](https://github.com/bigjust)
* [steenzout](https://github.com/steenzout) | [twitter](https:/twitter.com/steenzout)
* [jeancornic](https://github.com/jeancornic)
* [duhast](https://github.com/duhast)
* [kagux](https://github.com/kagux)
