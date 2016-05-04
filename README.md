sets up archivespace as a dev environment

# Dependencies
- [vagrant](https://www.vagrantup.com/downloads.html)
- [virtualbox](http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html)
- a terminal
- git
  
## Windows users
Windows users will need to obtain a linux-style terminal.
Kill two birds with one stone by installing [Git for Windows](https://git-scm.com/downloads); just be sure to install the 'git bash' component.

## Importing data
You can optionally import mysql data from an S3 bucket using credentials found in a file called aws_secrets. 
The expected mysqldump location is `s3://{{ bucket }}/latest/daily_archivesspace.sql.gz`.
To enable data import, edit the file `local.play`, setting the variable `fetch_data` to `True`, 

~~~
fetch_data: True
~~~

and replace the values in aws_secrets.example with your own, saving as `aws_secrets`.

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

To test the upgrade from v1.4.2 -> v1.5-rc1, first build with the 1.4.x branch, ensuring that your S3 info is set as described above and that the variable `fetch_data` is `true` and the variable `upgrade` is `false`. Once the build has completed, verify that your data appears as it should in the 1.4.2 version. Then, checkout branch 1.5.x and ensure that the variables just mentioned now have the opposite values. Building again, with `vagrant provision` most likely will download the new codebase after deleting the hsolr index and backing up the database as described in the upgrade instructions: https://github.com/archivesspace/archivesspace/blob/master/UPGRADING_1.5.0.md.

**vagrant workflow:**

	git clone --recursive https://github.com/lsulibraries/archivesspace_ansible.git
	git checkout 1.4.x
	vagrant up
	
	# time passes...
	# when complete, verify data via browser, check logs, etc...
	
	git checkout 1.5.x
	vagrant provision
	# when complete, verify data via browser, check logs, etc...	
