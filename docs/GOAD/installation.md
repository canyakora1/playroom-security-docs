---
title: GOAD - Installation
---

## Windows Installation (WSL)

## GOAD - Installation

```
Created by: Dcyberguy 
Created time: October 31, 2025 7:33 PM
Last edited by: Dcyberguy 
Last updated time: October 31, 2025 9:33 PM
Status: Done
```


## Installation

!!! warning "Choice of Operation System"

    I will be using the Windows Operating System and also Debian Linux (WSL) for this particular GOAD Install.
    The GOAD Repo would be on the Windows OS, same as with VMWare and VMware utility.

    The vagrant plugins would be installed from the WSL (Debian)

### Requirements:

- VMWare workstation:

Click below to install VMWare workstaion, if you don't already have it installed (Account needed to download)

[VM-Workstation](https://support.broadcom.com/group/ecx/productdownloads?subfamily=VMware+Workstation+Pro)

- Vagrant:

[Install | Vagrant | HashiCorp Developer](https://developer.hashicorp.com/vagrant/install#linux)
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://developer.hashicorp.com/vagrant/install" data-iframely-url="https://iframely.net/9huwoN2J?card=small&theme=dark"></a></div></div><script async src="https://iframely.net/embed.js"></script>


- Vagrant VMware utility:

[Install | Vagrant | HashiCorp Developer](https://developer.hashicorp.com/vagrant/install/vmware#linux)
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://developer.hashicorp.com/vagrant/docs/providers/vmware/vagrant-vmware-utility" data-iframely-url="https://iframely.net/n3Rha6Ua?card=small&theme=dark"></a></div></div><script async src="https://iframely.net/embed.js"></script>

- Clone the GOAD (Game of Active Directory GitHub Repo)

[GOAD](https://github.com/Orange-Cyberdefense/GOAD)
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/Orange-Cyberdefense/GOAD" data-iframely-url="https://iframely.net/vqnuYmSG?card=small&theme=dark"></a></div></div><script async src="https://iframely.net/embed.js"></script>

- Install the below vagrant plugins:

```bash
vagrant plugin install vagrant-reload vagrant-vmware-desktop winrm winrm-fs winrm-elevated
```

## On the WSL (Debian)

### Python3 Virtual env Install

I will be using a python virtual environment `venv` on the Debian WSL to deploy GOAD

```bash
cd /mnt/c/Users/dcyberguy/
sudo apt install python3.16-venv
python3 -m venv venv
source venv/bin/activate
cd /mnt/c/Users/dcyberguy/GOAD
```

### Check for dependencies

```bash
./goad.sh -p vmware
GOAD/vmware/local/192.168.56.X > check
```

## Install

- To install run the goad script and launch install or use the goad script arguments

```bash
./goad.sh -p vmware
# here choose the lab you want (GOAD/GOAD-Light/NHA/SCCM)
GOAD/vmware/local/192.168.56.X > set_lab <lab> 

# here choose the  ip range you want to use ex: 192.168.56 (only the first three digits)
GOAD/vmware/local/192.168.56.X > set_ip_range <ip_range> 

# Install
GOAD/vmware/local/192.168.56.X > install
```

At the end of your install, you have something like this:

![image.png](../assets/images/GOAD/installation.png)