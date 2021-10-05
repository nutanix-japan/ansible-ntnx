# ansible-ntnx

This is a sample repo to get started with Ansible integration with Windows. Although this is not a new concept. This is a repo to just share the folder strutucture to instlal Nutanix VirtIO with interested parties. 

There is a plan to increase the contents of this repo to include more Nutanix based use cases which can be achieved with Ansible.

Ansible uses a server and a client-less connection mechanism.

Ansible primarily uses the following pre-installed mechanisms to communicate

- For Linux like servers - SSH 
- For Windows servers - WinRM 

Ansible has a very easy to manage folder structure. Ansible is usually installed in ``/etc/ansible`` folder on your server.

/etc/ansible
├── ansible.cfg      
├── group_vars
│   └── windows.yml   <<  contains group parameters for connecting to a windows machines
├── hosts             <<  contains Ansible targets which can be managed individually or as a group 
├── VirtIO.yml   
└── roles


At a very high level we will be following in the setup to install Nutanix VirtIO MSI package on Windows Hosts.

- Installing Ansible server package on CentOS 8 server
- Setting up WinRM and associated components on Windows client machines
- Creating a Ansible hosts file to include all our target servers
- Running the Playbook for VirtIO 

