Host preparation
=========

Ansible role to prepare Ubuntu host.
- Configure hostname (optional)
- Configure apt-cacher-ng (optional)
- Configure user to can use sudo without password
- Update all packages include kernel to latest
- Install some tools (see [Role Variables](#Role-Variables))
- Tuning system (see [Role Variables](#Role-Variables))

Requirements
------------

Before using this role. Please prepare your public key to be authorized_keys and configure ```host_preparation_authorized_keys_path``` to point to your authorized_keys files.

Role Variables
--------------

```yaml
# This is default variables
host_preparation_reboot_timeout: 600
host_preparation_base_packages:
  - htop
  - iotop
  - sysstat
  - iftop
host_preparation_is_config_hostname: false

# This is optional variables
host_preparation_apt_cacher_ng: http://apt-cacher-ng.example.com:3142
```

Dependencies
------------

NA

Example Playbook
----------------

Since Ubuntu Xenial did't come with python 2 by default. So playbook need to install python 2 first without gathering facts.

```yaml
- hosts: all
  gather_facts: no
  become: true
  pre_tasks:
    - name: Install Python 2 first
      raw: python --version || apt update && apt install -y python
  roles:
    - ansible-host_preparation
  vars_files:
    - "{{ host_preparation_vars_file }}"
```

List of useful tags
----------------

There are some useful tags that you can use to maintain your Ubuntu host.

- host-preparation-apt-cacher-ng
- host-preparation-configure-hostname
- host-preparation-install-base-packages
- host-preparation-update-packages
- host-preparation-reboot (this need to configure ```host_preparation_need_reboot``` variable to true)

You can specific tag by using ```--tag``` for example if you only want to configure authorized_keys and limit to only production and database server group. You can run with command

```bash
ansible-playbook -i inventories/host_preparation --limit production:database \
--tag host-preparation-configure-authorized_keys host-preparation.yml
```

License
-------

MIT
