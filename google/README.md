#GCE Vagrant

To provision a cluster of virtual machines on google compute engine you have to follows these instructions

Not: Please do not hesitate to inform me if you have an issue with this readme file


1. Install Vagrant Google plugin

	$ vagrant plugin install vagrant-google

2. Configure Vagrantfile:

Vagrant uses vagrant file as a configuration file where user defines virtual machines, most of configuraions and Ruby code is written by vagrant gce team, so open the Vagrantfile and update these variables

	$GOOGLE_PROJECT_ID = 'YOUR GOOGLE PROJECT ID'
	$GOOGLE_CLIENT_EMAIL = 'YOUR CLIENT EMAIL'
	$GOOGLE_JSON_KEY_LOCATION = '/home/ubuntu/.ssh/privatekey-gce.json'
	$LOCAL_USER = 'ubuntu'
	$LOCAL_SSH_KEY = '/home/ubuntu/.ssh/id_rsa'

	To generate this <code>privatekey-gce.json</code> kindly follow these instructions [here](https://github.com/mitchellh/vagrant-google)
	<code>id_rsa</code> is your private key on your local machine.

	for each VM in Vagrant file update add your public key as shown below:

	z2c_zone.metadata = {"zone" => "US Central 1c", "sshKeys" => ['ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDnDFqXBeg6R/w5K9hMNhfb/f9Rt5AzFfjVB9XXXXXXXXXXXXXXXXXXXXXXXXXXXX ubuntu@cis5']}

3. Start the virtual machines
	
	$ vagrant up --provider google


4. Update hosts file

	you will find a sample hosts file in this directory .... for each node in cluster add the first node as 
	a master node "CAT server" and resource servers as slaves and their IPs.


5. Finally, Run Ansible playbook

	$  ansible-playbook -i google/hosts playbook.yml -vvvv

 
