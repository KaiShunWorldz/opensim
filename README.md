# ansible-ksw-02
## Status: :bangbang: alpha ( not finished )


Title : Ansible playbooks to generate a 3 server grid plus backups
        Opensim 0.9.2 with MySQL 8.0.2
==================================================================

These playbooks will install and manage a three server opensimulator grid.

( This is my first ansible project. Still learning.)

Servers:
  1) the grid server running the robust login service and the grid services
  2) the asset server, running the robust asset and inventory services
  3) the region server , running the simulator service
  4) the backup server receiving daily compressed full backups from the robust servers

Requirements
------------

- This setup requires three Ubuntu servers. The name of these servers are listed
  in the `infrastructure.cnf` file. The following setup has been used for this
  configuration making use of Oracle VirtualBox.
  - os-asset , 2 core, 3 GB Mem, 120 GB disk
  - os-login , 1 core, 1 GB Mem,  16 GB disk
  - os-region, 1 core, 4 Gb Mem,  60 GB disk
  - os-backup, 1 core, 1 Gb Mem, 100 GB disk

- The servers need to have python installed, and `/usr/bin/phython3` available


Configuration
------------
- The grid is configured by setting the variables in `group_vars/all.yml`
- You need to configure personal information such as usernames and passwords in
  the folder ~/.vars/opensim/ ( See roles/simulator/tasks/variables.yml )
- Port numbers for database backups are configures in `infrastructure.def`
- The asset server will run a mysql database. The database and the system
   account for the grid administrator are configured by setting the variables
   in `group_vars/robust/all.yml`
- The region server will run a mysql database to store simulator assets. The
  database and the system account for the region server administrator are
  configured by setting the variables in `group_vars/simulator/all.yml`    
- The passwords of the database are kept in a vault. You either generate a new
  vault for each group or define the vault_db_password variable in the groups_var
  file with the other variables. The location of the file containing the vault
  passphrase is configured in `ansible.cfg` ( Default contains 'VerySecret' )
- Compressed database backup files are send to the backup system. The ports that
  are used for these backups are configured in the file 'infrastructure.def'

  Roles
  ------
- control        : install optional tools for the control host
- baseline       : installs and configures packages common for all Servers
- mysql          : installs and configures mysql and the database as storage provider
                   for opensim  ( based on playbooks of geerlingguy
- opensim        : install opensimulator from github and setup of system accounts
- asset_services : configures the asset service and inventory service
- grid_services  : configures all grid services
- opensimulator  : configures the opensimulator             
- percona        : installs percona tools and percona xtrabackup and qpress
- db-backup      : schedules databases backups in `/etc/crontab`


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

Playbooks
----------
- site.yml      : runs the complete installation
- control.yml   : configures the control system
- baseline.yml  : installs common tools and mono
- opensim.yml   : configures all servers with the common settings and software
- storage.yml   : configures the storage provider mysql
- db-backup.yml : configures database backups
- asset.yml     : configures robust with the asset and inventory services
- grid.yml      : configures robust with the grid services
- simulator.yml : configures the simulators with the opensim services


- playbooks/
 -  info.yml          : shows basic information of a node in the network
 -  stack_status.yml  : shows service status of all services in the opensim stack
 -  stack_restart.yml : executes a controlled restart of the opensim stack

License
-------

BSD

Author Information
------------------
KaiShun Oleander

eMail  : kaishun@xs4all.nl

url    :http://www.kaishunworldz.com

Github : https://github.com/KaiShunOleander

Trello : https://trello.com/b/aSgcqTj2/opensimulator
