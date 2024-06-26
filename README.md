Ansible Role: SudoUsers
=======================

This role creates User Accounts from a list, with sudo previliges on Debian based OS or RHEL based OS. It will also add NOPASSWD flag so that password is not asked.

Requirements
------------

Any Debian or RHEL based Virtual Machine or Physical server where the Ansible user has SUDO permissions, and python3 is installed. Plus Package "sudo" must be installed.

Role Variables
--------------

For a complete list please see defaults/main.yml.
List of Users
```
su_users:
  - username: "{{ su_vault_vmuser1 }}"
    password: "{{ su_vault_vmpwd1 }}"
    is_super_user: true
    sudo_rules: []    
  - username: "{{ su_vault_vmuser2 }}"
    password: "{{ su_vault_vmpwd2 }}"
    is_super_user: false
    sudo_rules:
      - "ALL=(ALL)   NOPASSWD: /usr/bin/su - {{ su_vault_vmuser1 }}"
      - "ALL=(root)   NOPASSWD: /bin/su - {{ su_vault_vmuser1 }}"    
```
In the above example the first user is a Super user and sudo command is allowed (Super User). The 2nd user is a restricted user, with 2 sudo rules that allow him/her to switch to the 1st user using a command like:  
```
sudo su - user1id
```
In the above example for user2 only this 1 sudo command is allowed.
  
Secret User-names and Passwords should be stored in 'Secrets.yml'. Use
ansible-vault and Password hasing. Do not commit Secrets.yml to Git/Source Control.
```
su_vault_vmuser1: user01
su_vault_vmpwd1: user01-password-hashed
su_vault_vmuser2: user02
su_vault_vmpwd2: user02-password-hashed
```

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

## Linux User Password Hashing
### Ubuntu / Debian
```
$ sudo apt update
$ sudo apt install whois 
$ mkpasswd --method="sha-512" --salt="Thisisarandomsaltingstring"
Password: 
$6$ieMLxPFShvi6rao9$XEAU9ZDvnPtL.sDuSdRi6M79sgD9254b/0wZvftBNvMOjj3pHJBCIe04x2M.JA7gZ7MwpBWat1t4WQDFziZPw1
```
### CentOS / Fedora
```
$ sudo dnf install expect
$ mkpasswd --method="sha-512" --salt="Thisisarandomsaltingstring"
Password: 
$6$ieMLxPFShvi6rao9$XEAU9ZDvnPtL.sDuSdRi6M79sgD9254b/0wZvftBNvMOjj3pHJBCIe04x2M.JA7gZ7MwpBWat1t4WQDFziZPw1
```

License
-------

MIT

Author Information
------------------

This role was created on May 10, 2024 by [Hammad Rauf](https://www.linkedin.com/in/hammadrauf/).