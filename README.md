# Phansible Role: PHP Xdebug Extension

[![CI](https://github.com/phansible/role-php-xdebug/actions/workflows/ci.yml/badge.svg)](https://github.com/phansible/role-php-xdebug/actions/workflows/ci.yml)

An Ansible Role that installs and configures [Xdebug](https://xdebug.org/) 3 for PHP on Ubuntu and Debian, created with [Phansible](http://phansible.com/) in mind.

## Requirements

- Ubuntu 22.04, 24.04, 26.04 or Debian 12, 13 (x86_64)
- Ansible >= 2.15
- Root access or `become` on the target
- PHP installed by [role-php](https://github.com/phansible/role-php) earlier in the same play (PHP 8.2, 8.3, 8.4 and 8.5 are tested in CI)

## Role Variables

Defaults from `defaults/main.yml`:

| Variable | Default | Description |
| --- | --- | --- |
| `xdebug_settings` | see below | Map of Xdebug ini settings written to `/etc/php/<version>/mods-available/xdebug.ini` |

```yaml
xdebug_settings:
  xdebug.mode: debug
  xdebug.discover_client_host: 1
  xdebug.max_nesting_level: 300
```

Each key replaces an existing (or commented out) line for that setting, or is appended. Overriding `xdebug_settings` replaces the whole map.

The role also reads `php_version` from role-php to pick the `php<version>-xdebug` package.

## Dependencies

[role-php](https://github.com/phansible/role-php). It is not declared in `meta/main.yml`: run role-php before this role in the same play, since this role uses its `php_version` variable, its apt repository and its `restart php-fpm` handler.

## Example Playbook

```yaml
- hosts: all
  become: yes
  vars:
    xdebug_settings:
      xdebug.mode: debug,develop
      xdebug.client_host: 10.0.2.2
      xdebug.max_nesting_level: 300
  roles:
    - role-php
    - role-php-xdebug
```

## License

This role is released under the terms of the license specified in the repository or if not specified, under the [MIT license](https://raw.githubusercontent.com/phansible/role-php-xdebug/master/LICENSE).

## Author Information

This role was created in 2015 by [The Phansible Team & Contributors](https://github.com/phansible/role-php-xdebug/graphs/contributors), credits go also to [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/) and source of inspiration.
