# Ansible Automation for Nutanix Tools

This is a sample repo to get started with Ansible integration with Windows. Although this is not a new concept. This is a repo to just share the folder strutucture to instlal Nutanix VirtIO with interested parties. 

There is a plan to increase the contents of this repo to include more Nutanix based use cases which can be achieved with Ansible.

Ansible uses a server and a client-less connection mechanism.

- [Requirements](#requirements)
- [Ansible Folder Structure](#ansible-folder-structure)
- [Security Considerations](#security-considerations)
- [Install Ansible Serve](#install-ansible-server)
- [Setting up WinRM](#setting-up-winrm)
- [Ansible Hosts File](#ansible-hosts-file)
- [Ansible Playbook](#ansible-playbook)
- [Running Playbook](#running-playbook)
- [Closing Tips](#closing-tips)
## Requirements:

1 x Linux Server
Network connectivity to client machines
Internet or local repo connectivity on the Ansible server to download and install Ansible packages 

Ansible primarily uses the following pre-installed mechanisms to communicate:

For Linux like servers - SSH 
For Windows servers - WinRM 

## Ansible Folder Structure

Ansible has a very easy to manage folder structure. Ansible is usually installed in ``/etc/ansible`` folder on your server.

```

    /etc/ansible
    ├── ansible.cfg      
    ├── group_vars
    │   └── windows.yml   <<  contains group parameters for connecting to a windows machines
    ├── hosts             <<  contains Ansible targets which can be managed individually or as a group 
    ├── VirtIO.yml        <<  your playbook
    └── roles
```

## Security Considerations

It is a good idea to use Ansible Tower (licensed) to store your credentials for connecting to Ansible clients. Take care to not expose any credentials while storing informaion in public forums like Github.

At a very high level we will be following in the setup to install Nutanix VirtIO MSI package on Windows Hosts.

- Installing Ansible server package on CentOS 8 server
- Setting up WinRM and associated components on Windows client machines
- Creating a Ansible hosts file to include all our target servers
- Running the Playbook for VirtIO 

Now we will run through each step

## Install Ansible Server

Login (ssh) to your CentOS server and execute the following commands to install Ansible server.

This procedure assumes that you will be using a linux user with sudo permissions.

Installing pre-requisites and ansible:

```
  $ sudo yum update
  $ sudo install epel-release
  $ sudo yum install ansible
```

Installing PIP (pip is the package installer for Python) for WinRM:

```
  $ sudo pip3 install --upgrade setuptools
  $ python3 -m pip install --user --ignore-installed pywinrm
```
## Setting up WinRM




