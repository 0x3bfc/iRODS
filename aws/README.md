#AWS Vagrant

This readme file is showing how to start a cluster of virtual machines on AWS using Vagrant, kindly check out the following steps:

##Import vagrant box & Install AWS Vagrant

First you have to install AWS vagrant plugin then import the AWS box into vagrant (please see how to install vagrant from this [readme](https://github.com/aabdulwahed/iRODS) file)

    $ vagrant plugin install vagrant-aws
    $ vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box 

##Configure Vagrantfile

You have to add your <code>Access key</code>, <code>Secret key</code>, and <code>private key path</code>(absolute path) as shown below:

    aws.access_key_id = 'YOUR AWS ACCESS KEY'
    aws.secret_access_key = 'YOUR AWS SECRET KEY'
    .......
    #FOR EACH VM IN VAGRANT FILE MODIFY THE PATH OF YOUR PRIVATE KEY
    override.ssh.private_key_path = '/home/ubuntu/IRODS/iRODS/aws/keys/irods.pem'

##Start a cluster of virtual machines on AWS

    $ vagrant up --provider aws
    
Now you have two VMs, one is a master node that has the iCAT, and the second node is used for resources services.

##Create your hosts file

Hosts file will is used by ansible playbook as a target deployment infrastructure, so modify IPs as shown below

    [servers]
    master  ansible_ssh_port=22   ansible_ssh_host=52.91.253.0    ansible_ssh_user=ubuntu host_key_checking=false ansible_ssh_private_key_file=/home/ubuntu/IRODS/iRODS/aws/keys/irods.pem
    node-1  ansible_ssh_port=22   ansible_ssh_host=54.152.80.209    ansible_ssh_user=ubuntu host_key_checking=false ansible_ssh_private_key_file=/home/ubuntu/IRODS/iRODS/aws/keys/irods.pem
    [masters]
    master  ansible_ssh_port=22   ansible_ssh_host=52.91.253.0    ansible_ssh_user=ubuntu host_key_checking=false ansible_ssh_private_key_file=/home/ubuntu/IRODS/iRODS/aws/keys/irods.pem
    [slaves]
    node-1  ansible_ssh_port=22   ansible_ssh_host=54.152.80.209    ansible_ssh_user=ubuntu host_key_checking=false ansible_ssh_private_key_file=/home/ubuntu/IRODS/iRODS/aws/keys/irods.pem
    
# Start iRODS Deployment

Finally, we can start the deployment on AWS cluster using ansible-playbook as shown below:

    $ ansible-playbook -i aws/hosts playbook-cloud.yml -vvvv
