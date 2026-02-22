# [Ansible role zabbix_web](#ansible-role-zabbix_web)

Install and configure zabbix_web on your system.

|GitHub|GitLab|Downloads|Version|
|------|------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-zabbix_web/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-zabbix_web/actions)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-zabbix_web/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-zabbix_web)|[![downloads](https://img.shields.io/ansible/role/d/buluma/zabbix_web)](https://galaxy.ansible.com/buluma/zabbix_web)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-zabbix_web.svg)](https://github.com/buluma/ansible-role-zabbix_web/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-zabbix_web/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
- become: true
  gather_facts: true
  hosts: all
  name: Converge
  roles:
  - role: buluma.zabbix_web
    zabbix_web_groups:
    - name: Linux servers
    zabbix_web_hosts:
    - description: Example server 1 description
      groups:
      - Linux servers
      interface_dns: server1.example.com
      interface_ip: 192.168.127.127
      link_templates:
      - Template OS Linux by Zabbix agent
      name: Example server 1
      visible_name: Example server 1 name
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-zabbix_web/blob/master/molecule/default/prepare.yml):

```yaml
- become: true
  gather_facts: false
  hosts: all
  name: Prepare
  roles:
  - role: buluma.bootstrap
  - role: buluma.selinux
  - role: robertdebock.container_docs
  - role: buluma.buildtools
  - role: buluma.epel
  - role: buluma.python_pip
  - openssl_items:
    - common_name: '{{ ansible_fqdn }}'
      name: apache-httpd
    role: buluma.openssl
  - mysql_databases:
    - collation: utf8_bin
      encoding: utf8
      name: zabbix
    mysql_users:
    - name: zabbix
      password: zabbix
      priv: zabbix.*:ALL
    role: buluma.mysql
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
zabbix_web_database_name: zabbix
zabbix_web_database_pass: zabbix
zabbix_web_database_user: zabbix
zabbix_web_mysql_connection: socket
zabbix_web_password: zabbix
zabbix_web_server: '{{ ansible_fqdn }}'
zabbix_web_server_name: zabbix
zabbix_web_server_port: 10051
zabbix_web_username: Admin
zabbix_web_validate_certs: false
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-zabbix_web/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Build Status GitHub](https://github.com/buluma/ansible-role-buildtools/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-buildtools/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-buildtools)|
|[robertdebock.container_docs](https://galaxy.ansible.com/buluma/robertdebock.container_docs)|[![Build Status GitHub](https://github.com/buluma/robertdebock.container_docs/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.container_docs/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/robertdebock.container_docs/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/robertdebock.container_docs)|
|[buluma.ca_certificates](https://galaxy.ansible.com/buluma/ca_certificates)|[![Build Status GitHub](https://github.com/buluma/ansible-role-ca_certificates/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-ca_certificates/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-ca_certificates/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-ca_certificates)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Build Status GitHub](https://github.com/buluma/ansible-role-core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-core_dependencies/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-core_dependencies)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Build Status GitHub](https://github.com/buluma/ansible-role-epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-epel/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-epel/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-epel)|
|[buluma.httpd](https://galaxy.ansible.com/buluma/httpd)|[![Build Status GitHub](https://github.com/buluma/ansible-role-httpd/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-httpd/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-httpd/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-httpd)|
|[buluma.mysql](https://galaxy.ansible.com/buluma/mysql)|[![Build Status GitHub](https://github.com/buluma/ansible-role-mysql/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-mysql/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-mysql/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-mysql)|
|[buluma.openssl](https://galaxy.ansible.com/buluma/openssl)|[![Build Status GitHub](https://github.com/buluma/ansible-role-openssl/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-openssl/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-openssl/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-openssl)|
|[buluma.php](https://galaxy.ansible.com/buluma/php)|[![Build Status GitHub](https://github.com/buluma/ansible-role-php/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-php/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-php/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-php)|
|[buluma.python_pip](https://galaxy.ansible.com/buluma/python_pip)|[![Build Status GitHub](https://github.com/buluma/ansible-role-python_pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-python_pip/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-python_pip)|
|[buluma.selinux](https://galaxy.ansible.com/buluma/selinux)|[![Build Status GitHub](https://github.com/buluma/ansible-role-selinux/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-selinux/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-selinux/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-selinux)|
|[buluma.zabbix_repository](https://galaxy.ansible.com/buluma/zabbix_repository)|[![Build Status GitHub](https://github.com/buluma/ansible-role-zabbix_repository/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-zabbix_repository/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-zabbix_repository/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-zabbix_repository)|
|[buluma.zabbix_server](https://galaxy.ansible.com/buluma/zabbix_server)|[![Build Status GitHub](https://github.com/buluma/ansible-role-zabbix_server/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-zabbix_server/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-zabbix_server/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-zabbix_server)|

## [Context](#context)

This role is part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-zabbix_web/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|
|[opensuse](https://hub.docker.com/r/buluma/opensuse)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|
|[Kali](https://hub.docker.com/r/buluma/kalilinux)|all|

The minimum version of Ansible required is 2.12, tests have been done on:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them on [GitHub](https://github.com/buluma/ansible-role-zabbix_web/issues).

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-zabbix_web/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)

