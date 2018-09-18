sets up archivespace

## Vagrant
- first install by setting the playbook in the Vagrantfile as `ansible.playbook = "ansible/playbook-restore.yml"`
- once complete, add real values to the `ansible.extra_vars`, and then set `ansible.playbook = "ansible/playbook-restore.yml"`, followed by `vagrant provision`


# Dependencies
- [vagrant](https://www.vagrantup.com/downloads.html)
- [virtualbox](http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html)
- a terminal
- git

## Windows users
Windows users will need to obtain a linux-style terminal.
Kill two birds with one stone by installing [Git for Windows](https://git-scm.com/downloads); just be sure to install the 'git bash' component.

## Importing data
.....................


<<<<<<< HEAD
The playbook expects that you have created a file called `aws_secrets` that contains AWS credentials. Using the file `aws_secrets.example` as a starting point, place the placeholder values there with your own credentials and bucket name, saving the file as `aws_secrets`.
=======
>>>>>>> FB-prod

## Installation (dev box)
- open a terminal (git bash, on Windows)
- `git clone --recursive https://github.com/lsulibraries/archivesspace_ansible.git`
- `cd archivesspace_ansible`
- `vagrant up`


## Installation (production)
- be sure to provide commandline --extra_vars values for
  - `admin_network` - default is '0/0', all networks
  - `mysql_root_password` - default is 'root'
  - `archivesspace_hostname` - default is localhost; used for apache vhost configuration of the `Servername` parameter.

In either case, the installation process will take about 5-6 minutes during which you will see the output of the installation process scroll by in your terminal.
When the installation finishes, you should see output similar to the following:

	==> default:
	==> default: PLAY RECAP ********************************************************************
	==> default: localhost                  : ok=38   changed=23   unreachable=0    failed=0

While the installation will have finished, the application takes a minute or two to initialize.


## for the adventurous, a journey into the very dark place...

You can monitor its progress by tailing the archivespace log in the vm with `less`.
- `vagrant ssh`
- `sudo su`
- `less /opt/archivesspace/logs/archivesspace.out`
- Shift-F will take you to the 'tail' of the log
- `q` will exit the `less` buffer
- Ctrl-d twice will log you out of the vm

When your log displays output like the following, you should have full access to the application through your browser at localhost:<port-number>.

	************************************************************
	Welcome to ArchivesSpace!
	You can now point your browser to http://localhost:8080
	************************************************************


## Upgrade testing

To test the upgrade from v1.4.2 -> v1.5-rc1, first build ...... Once the build has completed, verify that your data appears as it should in the 1.4.2 version. Then, ........ Build again, with `vagrant provision` ............as described in the upgrade instructions: https://github.com/archivesspace/archivesspace/blob/master/UPGRADING_1.5.0.md.

**vagrant workflow:**

	git clone --recursive https://github.com/lsulibraries/archivesspace_ansible.git
	git checkout 1.4.x
	vagrant up

	# time passes...
	# when complete, verify data via browser, check logs, etc...

	git checkout 1.5.x
	vagrant provision
	# when complete, verify data via browser, check logs, etc...
