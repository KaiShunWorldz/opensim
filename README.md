# ansible role opensim
## Status: :bangbang: alpha ( not finished )


Title : Ansible playbooks to create a rubust or simulator server
================================================================

This role will configure a system as either a robust or
simulator server.


Requirements
------------

- Your infrastructure defenition has the following groups defined

```INI
[robust]
<server-list>

# All systems that will run simulator.exe
[simulator]
<region-server name>

# All systems which will have the opensimulator binaries installed
[opensim:children]
robust
simulator

# All systems that will offer the grid services
[grid_services]
<grid server name>

# All systems that will offer the asset and inventory service
[asset_services]
<asset server name>
```

- The servers need to have python installed, and `/usr/bin/phython3` available


Configuration
------------
The grid can be configured by setting the variables in your `group_vars/all.yml` An example can be downloaded from :link: [ansible-ksw-02](https://github.com/KaiShunOleander/ansible-ksw-02)

The variables are listed below along with the default values:

```YAML
host_version: os-ubuntu-2004
git_bin: https://github.com/KaiShunOleander/{{ host_version }}

admin_user: gridadmin
admin_group: opensim
conf_dir: config-default
```

`host_version`  is one of the opensim binanry distributions available on github. Which github account is set though the `git_bin` variable. These binaries are precompiled opensim versions for Linux platforms. 

`admin_user` and `admin_group` are the operating system user name and group.

`conf_dir` is the directory name where to store the .ini files for opensim. It is created under */home/{{ admin_user }}*

```YAML
db_name: opensim
db_user: gridadmin
db_password: gridadmin
```

MySQL database name and credentials opensim has to use. The creation of the database and its users is not part of this role. That should be done by using a mysql role.


```YAML
os_public_port: 8002
os_private_port: 8003
```

The public and private ports opensim has to use.

```YAML
WelcomeMessage: "Welcome, Spirit!!!"
gridname: "Ansible Generated Test Grid"
gridnick: "AnsibleTest"
gridmode: "grid"
```

The default welcome message, the name of the grid and the short nickname of the grid.

```YAML
db_name: opensim
db_user: gridadmin
db_password: gridadmin
```

 
Playbook tags
----------------
- packages           : upgrade / install needed packages
- configure          : configure all installed packages and opensim
- authorize          : only configure access authorizations
- authenticate       : only configure user account credentials
- simulator          : configure the simulators
- database_service   : configure the storage provider for openSim
- grid_services      : configure the grid services
- grid_login_service : configure login services for the grid
- asset_service      : configure the asset service


License
-------

BSD

Author Information
------------------
KaiShun Oleander

eMail  : kaishun@xs4all.nl

url    : http://www.kaishunworldz.com

Github : https://github.com/KaiShunOleander

Project: https://github.com/users/KaiShunOleander/projects/1
