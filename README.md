# [redis](#redis)

Install and configure redis on your system.

|Travis|GitHub|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-redis.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-redis)|[![github](https://github.com/robertdebock/ansible-role-redis/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-redis/actions)|[![quality](https://img.shields.io/ansible/quality/30211)](https://galaxy.ansible.com/robertdebock/redis)|[![downloads](https://img.shields.io/ansible/role/d/30211)](https://galaxy.ansible.com/robertdebock/redis)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-redis.svg)](https://github.com/robertdebock/ansible-role-redis/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.redis
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.apt_autostart
    - role: robertdebock.sysctl
      sysctl_items:
        - name: vm.overcommit_memory
          value: 1
    - role: robertdebock.grub
      grub_options:
        - option: transparent_hugepage
          value: never
```

For verification `molecule/resources/verify.yml` runs after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: no

  tasks:
    - name: check if connection still works
      ping:

    - name: write my_key to redis
      command: redis-cli set my_key my_value

    - name: read my_key from redis
      command: redis-cli get my_key
      register: redis_read_my_key_from_redis

    - name: show my_key
      debug:
        msg: "{{ redis_read_my_key_from_redis.stdout }}"

    - name: check value of my_key
      assert:
        that:
          - redis_read_my_key_from_redis.stdout == "my_value"
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.


## [Requirements](#requirements)

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.apt_autostart
- robertdebock.bootstrap
- robertdebock.epel
- robertdebock.sysctl
- robertdebock.grub

```

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/redis.png "Dependency")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|alpine|all|
|el|7, 8|
|debian|buster, bullseye|
|fedora|31, 32|
|opensuse|all|
|ubuntu|focal, bionic|

The minimum version of Ansible required is 2.9, tests have been done to:

- The previous version.
- The current version.
- The development version.



## [Testing](#testing)

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-redis) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-redis/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## [License](#license)

Apache-2.0


## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
