# iRODS
The Integrated Rule-Oriented Data System (iRODS) is open source data management software used by research organizations and government agencies worldwide. This package contains Ansible Scripts for Installation and Deployment of iRODS on a cluster on virtual machines.

# Configure iRODS

Before starting iRODS deployment, you have to generate some specific configuration keys and secrets, follow the next steps

**Install Vagrant**

This deployment runs over virtual machines provisioned by virtualbox, so first I am going to install virtualbox then install vagrant and import virtualbox box file to vagrant

install virtualbox

     $ sudo apt-get install virtualbox

install vagrant by downloading vagrant binaries from this [here](http://www.vagrantup.com/downloads.html), in our case, I am using Ubuntu 12.04 LTS 64 bits so I am going to install <code>vagrant_1.7.4_x86_64.rpm</code>

     $ sudo dpk -i vagrant_1.7.4_x86_64.rpm

**Install Ansible**

Installation of Ansible on Ubuntu 12.04 LTS

     $ sudo apt-get install software-properties-common
     $ sudo apt-add-repository ppa:ansible/ansible
     $ sudo apt-get update
     $ sudo apt-get install ansible

Other Linux Distributions

to install on another Linux Distribution check out [Ansible Docs](http://docs.ansible.com/intro_installation.html)

**Passwordless SSH**

Generate ssh private and public keys

    $ sudo apt-get install openssh-server
    $ ssh-keygen -t rsa 
    $ cp ~/.ssh/id_rsa* roles/common/files/


**Generate Required iRODS keys**

modify <code>group_vars/all</code> file

Generate zone key and modify the value of zone_key

    $ (openssl rand -base64 16 | sed 's,/,S,g' | sed 's,+,_,g' | cut -c 1-16 | tr -d '\n' ; echo "")

Generate negotiation key and modify the value of negotiation_key

    $ openssl rand -base64 32 | sed 's,/,S,g' | sed 's,+,_,g' | cut -c 1-32

Generate control plane key and modify the value of controle plane key

    $ openssl rand -base64 32 | sed 's,/,S,g' | sed 's,+,_,g' | cut -c 1-32


#Install iRODS

Once you install vagrant, ansible, and configure you keys and passwords, you able to install your cluster, but first start a new cluster using vagrant, in this package, you have Vagrantfile which creates three nodes one iCAT server and two resources servers, the same thing can be done if you have pre-defined physical servers in hosts file, what we are going to do now is to start cluster of virtual machines over virtualbox using vagrantfile, if you want to increase number of cluster just by editing the <code>Vagrantfile</code>.

Create Virtual Machines:

    $ vagrant up --provider virtualbox

Edit hosts file by modifying the hosts ip, username, and passwords for each server, then run ansible playbook

    $ ansible-playbook -i hosts playbook.yml -vvvv

**Test iDrobs-Web2**

iDrobs web2 is a web interface for iRODS:

    $ curl http://192.168.0.20:8080/login/login


#iRODS use cases:

* iRODS enables data discovery using a metadata catalog that describes every file, every directory, and every storage resource in the data grid.
* iRODS automates data workflows, with a rule engine that permits any action to be initiated by any trigger on any server or client in the grid.
* iRODS enables secure collaboration, so users only need to log in to their home grid to access data hosted on a remote grid.
* iRODS implements data virtualization, allowing access to distributed storage assets under a unified namespace, and freeing organizations from getting locked in to single-vendor storage solutions.
