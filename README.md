Ansible Role: SudoUsers
=======================

This role creates User Accounts from a list, with sudo previliges on Debian based OS or RHEL based OS. It will also add NOPASSWD flag so that password is not asked.

Requirements
------------

Any Debian or RHEL based Virtual Machine or Physical server where the Ansible user has SUDO permissions, and python3 is installed. Plus Package "sudo" must be installed.

Role Variables
--------------

For a complete list please see defaults/main.yml.

Dependencies
------------

None

Example Playbook
----------------

```
    - hosts: servers
      roles:
         - role: hammadrauf.sudousers
```

Testing with Molecule
---------------------
Launch Podman instances outside of Molecule/Ansible by using the following commands:
```
podman run -d --name debian12 --hostname debian12 -it docker.io/hammadrauf/dockerdeb12:latest sleep infinity & wait
podman run -d --name fedora40 --hostname fedora40 -it docker.io/hammadrauf/fedora40:latest
podman run -d --name ubuntu --hostname ubuntu -it docker.io/hammadrauf/ubuntunoble:latest sleep infinity & wait
```

License
-------

MIT

Author Information
------------------

This role was created on May 10, 2024 by [Hammad Rauf](https://www.linkedin.com/in/hammadrauf/).