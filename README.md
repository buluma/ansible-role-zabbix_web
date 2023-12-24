# [Ansible role zabbix_web](#zabbix_web)

Install and configure zabbix_web on your system.

|GitHub|Version|Issues|Pull Requests|
|------|-------|------|-------------|
|[![github](https://github.com/buluma/ansible-role-zabbix_web/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-zabbix_web/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-zabbix_web.svg)](https://github.com/buluma/ansible-role-zabbix_web/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-zabbix_web.svg)](https://github.com/buluma/ansible-role-zabbix_web/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-zabbix_web.svg)](https://github.com/buluma/ansible-role-zabbix_web/pulls/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-zabbix_web/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: buluma.zabbix_web
      # You can provision Zabbix groups.
      # Most options map directly to the documentation:
      # https://docs.ansible.com/ansible/latest/modules/zabbix_group_module.html
      zabbix_web_groups:
        - name: Linux servers
      # Add hosts to Zabbix.
      # Most options map directly to the documentation:
      # https://docs.ansible.com/ansible/latest/modules/zabbix_host_module.html
      zabbix_web_hosts:
        - name: Example server 1
          interface_ip: "192.168.127.127"
          interface_dns: server1.example.com
          visible_name: Example server 1 name
          description: Example server 1 description
          groups:
            - Linux servers
          link_templates:
            - Template OS Linux by Zabbix agent
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-zabbix_web/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: buluma.bootstrap
    - role: buluma.selinux
    - role: robertdebock.container_docs
    - role: buluma.buildtools
    - role: buluma.epel
    - role: buluma.python_pip
    - role: buluma.openssl
      openssl_items:
        - name: apache-httpd
          common_name: "{{ ansible_fqdn }}"
    - role: buluma.mysql
      mysql_databases:
        - name: zabbix
          encoding: utf8
          collation: utf8_bin
      mysql_users:
        - name: zabbix
          password: zabbix
          priv: "zabbix.*:ALL"
    - role: buluma.php
    - role: buluma.httpd
    - role: buluma.ca_certificates
    - role: buluma.zabbix_repository
    - role: buluma.core_dependencies
    - role: buluma.zabbix_server
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-zabbix_web/blob/master/defaults/main.yml):

```yaml
---
# defaults file for zabbix_web

# How to connect to the mysql database, either `socket` or `network`.
zabbix_web_mysql_connection: socket

# Details to connect to the database.
zabbix_web_database_name: zabbix
zabbix_web_database_user: zabbix
zabbix_web_database_pass: zabbix

# When `zabbix_web_mysql_connection` is set to `network` this role needs
# these extra setting.
# zabbix_web_database_host: localhost
# zabbix_web_database_port: 3306

# Details to connect to Zabbix.
zabbix_web_server: "{{ ansible_fqdn }}"
zabbix_web_server_port: 10051
zabbix_web_server_name: zabbix

# Details to connect to the Zabbix API.
zabbix_web_username: Admin
zabbix_web_password: zabbix
zabbix_web_validate_certs: no
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-zabbix_web/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Ansible Molecule](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-buildtools.svg)](https://github.com/shadowwalker/ansible-role-buildtools)|
|[robertdebock.container_docs](https://galaxy.ansible.com/buluma/robertdebock.container_docs)|[![Ansible Molecule](https://github.com/buluma/robertdebock.container_docs/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/robertdebock.container_docs/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/robertdebock.container_docs.svg)](https://github.com/shadowwalker/robertdebock.container_docs)|
|[buluma.ca_certificates](https://galaxy.ansible.com/buluma/ca_certificates)|[![Ansible Molecule](https://github.com/buluma/ansible-role-ca_certificates/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-ca_certificates/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-ca_certificates.svg)](https://github.com/shadowwalker/ansible-role-ca_certificates)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Ansible Molecule](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-core_dependencies.svg)](https://github.com/shadowwalker/ansible-role-core_dependencies)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Ansible Molecule](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-epel.svg)](https://github.com/shadowwalker/ansible-role-epel)|
|[buluma.httpd](https://galaxy.ansible.com/buluma/httpd)|[![Ansible Molecule](https://github.com/buluma/ansible-role-httpd/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-httpd/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-httpd.svg)](https://github.com/shadowwalker/ansible-role-httpd)|
|[buluma.mysql](https://galaxy.ansible.com/buluma/mysql)|[![Ansible Molecule](https://github.com/buluma/ansible-role-mysql/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-mysql/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-mysql.svg)](https://github.com/shadowwalker/ansible-role-mysql)|
|[buluma.openssl](https://galaxy.ansible.com/buluma/openssl)|[![Ansible Molecule](https://github.com/buluma/ansible-role-openssl/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-openssl/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-openssl.svg)](https://github.com/shadowwalker/ansible-role-openssl)|
|[buluma.php](https://galaxy.ansible.com/buluma/php)|[![Ansible Molecule](https://github.com/buluma/ansible-role-php/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-php/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-php.svg)](https://github.com/shadowwalker/ansible-role-php)|
|[buluma.python_pip](https://galaxy.ansible.com/buluma/python_pip)|[![Ansible Molecule](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-python_pip.svg)](https://github.com/shadowwalker/ansible-role-python_pip)|
|[buluma.selinux](https://galaxy.ansible.com/buluma/selinux)|[![Ansible Molecule](https://github.com/buluma/ansible-role-selinux/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-selinux/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-selinux.svg)](https://github.com/shadowwalker/ansible-role-selinux)|
|[buluma.zabbix_repository](https://galaxy.ansible.com/buluma/zabbix_repository)|[![Ansible Molecule](https://github.com/buluma/ansible-role-zabbix_repository/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-zabbix_repository/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-zabbix_repository.svg)](https://github.com/shadowwalker/ansible-role-zabbix_repository)|
|[buluma.zabbix_server](https://galaxy.ansible.com/buluma/zabbix_server)|[![Ansible Molecule](https://github.com/buluma/ansible-role-zabbix_server/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-zabbix_server/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-zabbix_server.svg)](https://github.com/shadowwalker/ansible-role-zabbix_server)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-zabbix_web/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Debian](https://hub.docker.com/repository/docker/buluma/debian/general)|bullseye|
|[opensuse](https://hub.docker.com/repository/docker/buluma/opensuse/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|focal, bionic|
|[Kali](https://hub.docker.com/repository/docker/buluma/kali/general)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-zabbix_web/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-zabbix_web/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-zabbix_web/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)


### [Special Thanks](#special-thanks)

Template inspired by [Robert de Bock](https://github.com/robertdebock)
