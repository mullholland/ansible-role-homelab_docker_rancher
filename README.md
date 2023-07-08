# [homelab_docker_rancher](#homelab_docker_rancher)

Installs a docker-compose/systemd service for the [Rancher Server](https://ranchermanager.docs.rancher.com/pages-for-subheaders/deploy-rancher-manager) container.

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-homelab_docker_rancher/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-homelab_docker_rancher/actions)|[![gitlab](https://gitlab.com/opensourceunicorn/ansible-role-homelab_docker_rancher/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-homelab_docker_rancher)|[![quality](https://img.shields.io/ansible/quality/)](https://galaxy.ansible.com/mullholland/homelab_docker_rancher)|[![downloads](https://img.shields.io/ansible/role/d/)](https://galaxy.ansible.com/mullholland/homelab_docker_rancher)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-homelab_docker_rancher.svg)](https://github.com/mullholland/ansible-role-homelab_docker_rancher/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-homelab_docker_rancher/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  # vars:
  #   example_var: "value"
  roles:
    - role: "mullholland.homelab_docker_rancher"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-homelab_docker_rancher/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true

  roles:
    - name: mullholland.docker

  post_tasks:
    - name: check if connection still works
      ansible.builtin.ping:
```


## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-homelab_docker_rancher/blob/master/defaults/main.yml):

```yaml
---
# --------------------------------------
# General config
# --------------------------------------
rancher_base_path: "/opt/rancher"
rancher_sub_path:
  - "data"
  - "auditlog"
  - "certs"
  - "backup"
rancher_timezone: "Europe/Berlin"

# --------------------------------------
# User/Group
# --------------------------------------
# User/Group of the stack. Everything is mapped to this, instead of root.
rancher_user: "rancher"
rancher_group: "rancher"
# this user/group be a system user
rancher_user_system: true

# --------------------------------------
# rancher
# --------------------------------------
rancher_enable: true
rancher_image: "rancher/rancher:latest"

# Configure the container
rancher_compose:
  rancher:
    container_name: "rancher"
    image: "{{ rancher_image }}"
    restart: always
    user: "{{ rancher_user }}:{{ rancher_group }}"
    privileged: true
    environment:
      TZ: "{{ rancher_timezone }}"
      AUDIT_LEVEL: "1"
      # SSL_CERT_DIR: "/container/certs"
    command:
      - '--config.file=/etc/rancher/rancher.yml'
      - '--storage.path=/rancher'
    ports:
      - 80:80
      - 443:443
    volumes:
      - "{{ rancher_base_path }}/rancher/data:/var/lib/rancher"
      - "{{ rancher_base_path }}/rancher/auditlog:/var/log/auditlog"
      # - "{{ rancher_base_path }}/rancher/certs:/container/certs"
      # - "{{ rancher_base_path }}/rancher/backup:/backup"
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-homelab_docker_rancher/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[mullholland.docker](https://galaxy.ansible.com/mullholland/docker)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-docker/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-docker/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-docker/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-docker)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-homelab_docker_rancher/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/repository/docker/mullholland/docker-centos-systemd/general)|all|
|[Amazon](https://hub.docker.com/repository/docker/mullholland/docker-amazonlinux-systemd/general)|Candidate|
|[Fedora](https://hub.docker.com/repository/docker/mullholland/docker-fedora-systemd/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/mullholland/docker-ubuntu-systemd/general)|all|
|[Debian](https://hub.docker.com/repository/docker/mullholland/docker-debian-systemd/general)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-homelab_docker_rancher/issues)

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-homelab_docker_rancher/blob/master/LICENSE).

## [Author Information](#author-information)

[Mullholland](https://mullholland.net)

Please consider [sponsoring me](https://github.com/sponsors/mullholland).
