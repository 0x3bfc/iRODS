#GCE Vagrant

To provision a cluster of virtual machines on google compute engine you have to follows these instructions

Not: Please do not hesitate to ask if you have an issue with this readme file


#Import vagrant box & Install Vagrant Google plugin#

      $ vagrant box add gce https://github.com/mitchellh/vagrant-google/raw/master/google.box
      $ vagrant plugin install vagrant-google

#Configure Vagrantfile:

Vagrant uses vagrant file as a configuration file where user defines virtual machines. Most of configuraions showed in Vagrantfile are written by vagrant gce team, so to start a VM cluster on Google Compute Engine, just open the Vagrantfile and update the following variables:


	$GOOGLE_PROJECT_ID = 'YOUR GOOGLE PROJECT ID'
	$GOOGLE_CLIENT_EMAIL = 'YOUR CLIENT EMAIL'
	$GOOGLE_JSON_KEY_LOCATION = '/home/ubuntu/.ssh/privatekey-gce.json'
	$LOCAL_USER = 'ubuntu'
	$LOCAL_SSH_KEY = '/home/ubuntu/.ssh/id_rsa'

To generate this <code>privatekey-gce.json</code> kindly follow these instructions [here](https://github.com/mitchellh/vagrant-google). The <code>id_rsa</code> is your private key on your local machine.

Get your public key:

        $ cat ~/.ssh/id_rsa.pub

For each VM in Vagrant file, add your public key as shown below:

        z2c_zone.metadata = {"zone" => "US Central 1c", "sshKeys" => ['ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDnDFqXBeg6R/w5K9hMNhfb/f9Rt5AzFfjVB9XXXXXXXXXXXXXXXXXXXXXXXXXXXX ubuntu@cis5']}

#Start the virtual machines
	
        $ vagrant up --provider google


#Update hosts file

you will find a sample hosts file in this directory .... for each node in cluster add the first node as a master node "CAT server" and resource servers as slaves and their IPs.


#Finally, Run Ansible playbook

	$  ansible-playbook -i google/hosts playbook.yml -vvvv

 
