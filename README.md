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

Ansible has an agent less connectivity Mechanism. It uses basic OS features (SSH and WinRM) to communicate with client machines.

1. 1 x Linux Server
2. Network connectivity to client machines
    - WinRM connectivity for Windows servers
    - SSH connectivity for Linux servers
3. Internet or local repo connectivity on the Ansible server to download and install Ansible packages 

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

### Ansible Secrets

It is a good idea to use Ansible Tower (licensed) to store your credentials for connecting to Ansible clients. Take care to not expose any credentials while storing informaion in public forums like Github.

### WinRM Enablement Script

The script used in this article is a a direct download from a contributor in Public Github. 

View the script here before downloading and executing [WinRM Enablement Script](https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1).

It is advised to go through this script carefully before executing this on your client machines. We do not provide any support for this script nor take responsibility for the effects of running this script.

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

This should get your Ansible server to communicate with Windows clients. 
## Setting up WinRM

If your Windows clients are not installed, run the following script in PowerShell on your Windows client to enable the following:

- WinRM Listener (TCP 5895 and TCP 5896)
- WinRM Service

It is important that your firewall is open inbound for these services.

You only need to complete setup in this section if your Windows clients do not have WinRM enabled.

Run the following script in PowerShell on the client Windows servers. 

```

[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12

$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"
(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file

winrm get winrm/config

```
You can also run the script using PowerShell remotely. Please be sure to check out the pre-requistes for doing this.

Here is an example of how you would run PowerShell script remotely.

```
Invoke-Command -ComputerName Server01, Server02 -FilePath C:\ConfigureRemotingForAnsible.ps1

```

This is another way of running your script remotely.

```
Invoke-Command -ComputerName Server01, Server02 -ScriptBlock
{
Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12

$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"
(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file

winrm get winrm/config
}
```

Once you have done this all target windows VMs you are ready to move to next step.










