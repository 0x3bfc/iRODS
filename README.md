# iRODS
The Integrated Rule-Oriented Data System (iRODS) is an open source data management software used by research organizations and government agencies worldwide. This package contains Ansible Scripts for Installation and Deployment of iRODS on a cluster of virtual machines.

![Alt text] (images/iRODS-Logo.png "iRODS")

# Configure iRODS

Before starting iRODS deployment, you have to generate some specific configuration keys, secrets and install reuired packages, follow the next steps

**Install Vagrant**

This deployment runs over virtual machines provisioned by virtualbox, so first I am going to install virtualbox then install vagrant to start our cluster.

install virtualbox

     $ sudo apt-get install virtualbox

Install vagrant by downloading vagrant binaries from this [here](http://www.vagrantup.com/downloads.html), in our case, I am using Ubuntu 12.04 LTS 64 bits as a Host OS, so I am going to install <code>vagrant_1.7.4_x86_64.rpm</code>

     $ sudo dpk -i vagrant_1.7.4_x86_64.rpm

**Install Ansible**

Installation of Ansible on Ubuntu 12.04 LTS, Ansible is an automation tool to run your scipts over your infrastructure.

     $ sudo apt-get install software-properties-common
     $ sudo apt-add-repository ppa:ansible/ansible
     $ sudo apt-get update
     $ sudo apt-get install ansible

Other Linux Distributions

If you have another Linux Distribution or Operating System, kindly check out [Ansible Docs](http://docs.ansible.com/intro_installation.html)

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

Once you install vagrant, ansible, and your keys and passwords are created, you are able to install iRODS over your cluster, but before that you have to start a new cluster using vagrant, in this package, you have Vagrantfile which creates three nodes one iCAT server containes (iCAT and iDROP-WEB2) and two resources servers, the same thing can be done if you have pre-defined physical servers in hosts file "<code>hosts</code>", what we are going to do now is to start cluster of virtual machines using virtualbox and vagrantfile, if you want to scale up your cluster just edit the <code>Vagrantfile</code> and <code>hosts</code> file.

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
