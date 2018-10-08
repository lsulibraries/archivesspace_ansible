# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/xenial64"

  config.vm.network "forwarded_port", guest: 80, host: 8000
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 8089, host: 8089
  config.vm.network "forwarded_port", guest: 8081, host: 8081
  config.vm.network "forwarded_port", guest: 8090, host: 8090
  config.vm.network "forwarded_port", guest: 3306, host: 3306


  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
  end

  # Taken from https://github.com/geerlingguy/JJG-Ansible-Windows:
  # Use rbconfig to determine if we're on a windows host or not.
  require 'rbconfig'
  is_windows = (RbConfig::CONFIG['host_os'] =~ /mswin|mingw|cygwin/)
  if is_windows
    config.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible/playbook-fresh_install.yml"
      ansible.verbose = 'vv'
      ansible.install = true
      ansible.extra_vars = {
        archivesspace_hostname: "192.168.33.10",
        archivesspace_ssl_files: "no",
        archivesspace_ssl_selfsigned: "yes",
        s3_sync_aws_access_key_id: "abcdefg",
        s3_sync_aws_secret_access_key: "somethinglong+confusing",
        s3_sync_bucket: "mybucket",
        s3_sync_folder: "backup/"
      }
    end
  else
    # Provisioning configuration for Ansible (for Mac/Linux hosts).
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/playbook-proxy.yml"
      ansible.verbose = 'vv'
      ansible.extra_vars = {
        archivesspace_hostname: "192.168.33.10",
        archivesspace_ssl_files: "no",
        archivesspace_ssl_selfsigned: true,
        s3_sync_bucket: "mybucket",
        s3_sync_folder: "backup/",
        archivesspace_restore_s3_access_key_id: "keyidgoeshere",
        archivesspace_restore_s3_secret_access_key: "sthsecret+keylike",
      }
    end
  end
end
