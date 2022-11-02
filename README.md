# kyndryl-training-task

This repository contains solution for assigned homework. 
General information for presented solution is given here, more detailed parts are included in comments throughout the playbooks.

### Testing Environment
- RHEL 9.0 for Control Node and Managed Nodes (two nodes) which were running in VirtualBox 6.1.26
- it was neccessary to register RHEL systems in order to allow download and installation of packages via yum
- RHEL systems have 1 network adapter configured as bridge to host network 
- Ansible Core 2.13.5 with default settings
- Python 3.9.10 with default settings, preinstalled on RHEL
- VS Code 1.72.2 as primary IDE with Remote-SSH extension to edit files directly on Control Node

### Task approach/steps
**Prepare Control Node**
1. install pip on Control Node
2. install Ansible for user `devops` on Control Node
3. add IP addresses of Managed Nodes into `/etc/ansible/hosts` file and group them as `homework`
4. create `group_vars` and `host_vars` for specific playbooks

**Task #0: Prepare Managed Nodes**
1. install `sshpass` via yum to successfully run initial playbook<br>
   - assuming clean install of RHEL distro with root login over SSH enabled
2. run `initial-setup.yml` playbook to prepare Managed Nodes<br>
   - use `--ask-pass` parameter to get prompt for password of root user

**Task #1: Deploy LAMP stack on each server and single PHP page**
1. run `lamp.yml` to install basic LAMP stack and do basic configuration

**Task #2: Deploy users on server based on provided CSV source and setup user login**
1. install `community.general` collection to Ansible in order to use `read_csv` module
2. run `add-users-from-csv.yml` to find and read correct .csv file and add specified users<br>
   - use `--vault-id admin_password@passwords.txt` parameter<br>
   - password for default admin account is `loremipsum`
   - _vault passwords (in `passwords.txt`) would be most likely handled in a different way in production environment, this is just for clarity and ease of access_
3. to verify SSH connection of added users, use `sudo ssh -i /etc/ansible/keys/id_user_rsa user@node-ipaddress`<br>
   - where `user` is one of the users from .csv file

**Task #3: Modify hosts-file**
1. run `set-hosts.yml` to update `/etc/hosts` on each Managed Node with IP addresses of every other Managed Node
