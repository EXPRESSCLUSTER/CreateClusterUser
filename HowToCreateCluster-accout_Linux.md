# How to create cluster accout for Linux

## Overview
This article shows how to install, configure and operate EXPRESSCLUSER with an user account other than root account on Linux.

## Preparation with root account
### On all nodes
1. Logon with root accout.
1. Create user-gourp "clp" and user "clpadm" to install EXPRESSCLUSTER and configure/operation cluster.
```bat
# groupadd clp
# useradd -g clp clpadm
# passwd clpadm
  <Set clp user password>
```
1. Add permission to "clpadm" to execute commands.
```bat
# visudo
clpadm ALL=NOPASSWD: /sbin/reboot
clpadm ALL=NOPASSWD: /usr/bin/rpm
clpadm ALL=NOPASSWD: /usr/sbin/clp*
```

### Install EXPRESSCLUSTER 
On all nodes
1. Logon with clpadm account
1. Install EXPRESSCLUSTER, register its license and reboot the node.  
	```bat
	$ sudo rpm -i expresscls-3.3.5-1.x86_64.rpm
	$ sudo rpm -qa | grep express
	expresscls-3.3.5-1.x86_64
	$ sudo clplcnsc -i ECX3.x-lin.key -p BASE33
	$ sudo clplcnsc -v

	< Cluster License EXPRESSCLUSTER X 3.3 for Linux <TRIAL> >
	Seq... 1
	   Key..................... XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX
	   User name............... NEC
	   Start date.............. 2019/04/01
	   End date................ 2020/03/30
	   Status.................. valid
	$ sudo reboot
	```
### Create cluster configuration
#### (a) In the case of using WebManager or Cluster WebUI
On client machine
1. Connect to WebManager or Cluster WebUI.
1. Create a new cluster and appy it.
1. If there are exec resources or genw monitor resources, scripts which are used by them (start.sh/stop.sh/gens.sh) should be executable by root.  
	Check their permission.

#### (b) In the case of importing existing clp.conf
On primary node
1. Login with clpadm account.
1. Store clp.conf.
	```bat
	e.g.) /home/clpadm/clp.conf
	```
1. Apply the config.
	```bat
	$ sudo clpcfctrl --push -l/w -x /home/clpadm
	``` 
1. If there are exec resources or genw monitor resources, scripts which are used by them (start.sh/stop.sh/gens.sh) should be executable by root.  
	Check their permission.

### Start failover group and check it's status
On primary node
1. Login with clpadm account.
1. Start failover group and it's statu or behaviour
	```bat
	$ sudo clpgrp -s
	$ sudo clpstat
	$ sudo clpgrp -m
	$ sudo clpstat
	$ sudo clpgrp -m -h <secondary node name>
	$ sudo clpstat	
	```
