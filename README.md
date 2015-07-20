# iRODS
Ansible Scripts for Installation and Deployment of IRODS

# Configure iRODS

Before start iRODS deployment, you have to generate some specific configuration keys and secrets, follow next steps

**Passwordless SSH**

Generate ssh private and public keys

    $ sudo apt-get install openssh-server
    $ ssh-keygen -t rsa 
    $ cp ~/.ssh/id_rsa* roles/common/files/


**Generate Required iRODS keys**

modify group_vars/all file

Generate zone key and modify the value of zone_key

    $ (openssl rand -base64 16 | sed 's,/,S,g' | sed 's,+,_,g' | cut -c 1-16 | tr -d '\n' ; echo "")

Generate negotiation key and modify the value of negotiation_key

    $ openssl rand -base64 32 | sed 's,/,S,g' | sed 's,+,_,g' | cut -c 1-32

Generate control plane key and modify the value of controle plane key

    $ openssl rand -base64 32 | sed 's,/,S,g' | sed 's,+,_,g' | cut -c 1-32

#iRODS use cases:

* iRODS enables data discovery using a metadata catalog that describes every file, every directory, and every storage resource in the data grid.
* iRODS automates data workflows, with a rule engine that permits any action to be initiated by any trigger on any server or client in the grid.
* iRODS enables secure collaboration, so users only need to log in to their home grid to access data hosted on a remote grid.
* iRODS implements data virtualization, allowing access to distributed storage assets under a unified namespace, and freeing organizations from getting locked in to single-vendor storage solutions.
