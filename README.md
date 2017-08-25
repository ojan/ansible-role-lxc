Ansible Role: LXC
=========

Creation of a LXC container through the LXD API with Ansible [lxd-module](docs.ansible.com/ansible/lxd_container_module.html). It also provision python, sudo, ssh and user ssh key to have a fully ready Ansible controlable container.

Requirements
------------

A working installation of LXD with some remote. Example `lxc remote list` command output below:


```
+-----------------+------------------------------------------+---------------+--------+--------+
|      NAME       |                   URL                    |   PROTOCOL    | PUBLIC | STATIC |
+-----------------+------------------------------------------+---------------+--------+--------+
| images          | https://images.linuxcontainers.org       | simplestreams | YES    | NO     |
+-----------------+------------------------------------------+---------------+--------+--------+
| lxd-00          | https://127.0.1.1:8443                   | lxd           | NO     | NO     |
+-----------------+------------------------------------------+---------------+--------+--------+
| lxd-01          | https://192.168.1.1:8443                 | lxd           | NO     | NO     |
+-----------------+------------------------------------------+---------------+--------+--------+
| ubuntu          | https://cloud-images.ubuntu.com/releases | simplestreams | YES    | YES    |
+-----------------+------------------------------------------+---------------+--------+--------+
| ubuntu-daily    | https://cloud-images.ubuntu.com/daily    | simplestreams | YES    | YES    |
+-----------------+------------------------------------------+---------------+--------+--------+
```

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

- `lxc_profiles`: Profiles to use for the container. Must be created before used. Default to `['default']`
- `lxc_container_distro`: The Linux image distribution to launch in the container. Default to `16.04/amd64`
- `lxc_image_source`: The image source to use for downloading images. Default to `https://cloud-images.ubuntu.com/releases`
- `lxc_lxd_host`: The LXD hostname where to boot the container. Must be listed in `lxc remote list` command. Default to `lxd-01`
- `lxc_lxd_port`: The LXD port to contact. Default to `8443`
- `lxc_client_key`: The lxc client key used to contact the LXD daemon. Default to `{{ lookup('env', 'HOME') }}/.config/lxc/client.key`
- `lxc_client_cert`: The lxc client certificate used to contact the LXD daemon. Default to `{{ lookup('env', 'HOME') }}/.config/lxc/client.crt`
- `lxc_user_ssh_key`: The ssh user key to be used for operations in container with Ansible. Default to `{{ lookup('file', lookup('env','HOME') + '/.ssh/id_ed25519.pub') }}`


Dependencies
------------

None

Example Playbook
----------------

First, set an inventory file or with host/group_vars necessary variables like: 

```
[containers]
lxc-00 lxc_lxd_host=lxd-01
lxc-01 lxc_lxd_host=lxd-02
```

Where `lxc_lxd_host` is th eLXD host where you want yout LCX container to run.

Then set as usual a playbook like this one: 

    - hosts: containers
      gather_facts: false
      roles:
         - { role: ojan.lxc-node }

Set `gather_facts` to `false` to avoid the setup module to run because you can't ssh connect in a non-existent container.

License
-------

MIT

Author Information
------------------

This role was created in 2017 by Olivier Jan.
