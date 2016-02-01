## Environments

### Production
Running in aws at aspace.lib.lsu.edu. This is the authoritative, public-facing instance with persistent data.
Backups are taken daily in the early morning hours. At any given time, there are 

  * daily snapshots going three days into the past, 
  * weekly snapshots going back three weeks, and 
  * monthly snapshots going back three months
  
Backups are made via cron using the script provided with archivesspace. http://archivesspace.github.io/archivesspace/user/backup-and-recovery/

### Testing
Running at libaspace.lsu.edu. This instance exists primarily to test upgrades (and batch imports ???). 
This instance need not be persistent; this instance can be created quickly whenever the need arises.
Data and configuration will be imported from the production system.


### Development (Vagrant)
Local development instances can be created as needed using vagrant (see README.md for instructions).
The Vagrantfile can reflect production or a new version to be tested.

## Upgrade procedure

### on Testing

  * data and config imported to testing instance from production system
  * run the ansible playbook against the testing instance, making sure to specify the new version https://github.com/lsulibraries/archivesspace_ansible/blob/master/ansible/roles/archivesspace/vars/main.yml
  * manual testing
  * if all is well in the upgraded testing instance, the upgrade can proceed on the production system.

### on Production
  * run the backup script
  * run the ansible playbook against the production instance, making sure to update (and commit/push) the archivesspace role vars to use the new version https://github.com/lsulibraries/archivesspace_ansible/blob/master/ansible/roles/archivesspace/vars/main.yml
  * manual testing
