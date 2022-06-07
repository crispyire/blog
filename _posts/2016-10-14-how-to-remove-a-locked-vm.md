---
id: 553
title: 'How to remove a locked VM'
date: '2016-10-14T20:29:35+01:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=553'
permalink: /2016/10/how-to-remove-a-locked-vm/
categories:
    - VMware
tags:
    - ESXCLI
    - esxi
    - vmware
    - vsphere
---

I was asked by one of my clients to look at a locked VM that couldn’t be powered off, deleted, edited, vMotioned or otherwise altered from its current state.

![locked VM](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/10/poweroff-1.png?resize=664%2C56)

As the client wasn’t able to do some basic troubleshooting steps such as evacuating the host, restarting services or rebooting hosts due to current restrictions in the environment, I was asked what could be done to ‘unlock’ the VM so it could be edited or deleted.

This was a vCenter 5.5 with ESXi 5.1 hosts but the procedure below should work on all versions of ESXi.

Firstly, identify the host the VM is currently residing on:

![VM Summary](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/10/VM_Summary.png?resize=300%2C277)  
…and enabling SSH access to that host:

![Enable SSH](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/10/SSH.png?resize=300%2C150)

…and then SSH into that host.

![ssh_putty](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/10/SSH_Putty.png?resize=300%2C100)

After logging in, issue the command:

```
<pre class="brush: bash; title: ; notranslate" title="">
esxcli vm process list
```

This will list the VMs on the host and most importantly the VM’s World ID and its storage location of the VMX File.

![world-id](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/10/World-ID.png?resize=300%2C58)  
After confirming that the VM is the one with the issue, you can power off the VM with the command:

```
<pre class="brush: bash; title: ; notranslate" title="">
esxcli vm process kill -t [soft,hard,force] -w WorldNumber
```

so for example:

```
<pre class="brush: bash; title: ; notranslate" title="">
esxcli vm process kill -t hard -w 123456789
```

NB.

- Soft = Graceful
- Hard = Immediate shutdown
- Force = Nuclear option

Once the VM has been powered down, you can then remove it from the vCenter inventory. A clean-up operation was then completed on the files to free up the storage (By taking the path name taken from the esxcli output and removing the files and folder).

As this environment has HA enabled, it did trigger HA to try and bring the VM up on another host.

![ha_failover_failed](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/10/ha_failover_failed.png?resize=555%2C111)

In our case it failed to do so (Not unexpected), so after reviewing [KB2004802](https://kb.vmware.com/kb/2004802), HA was turned off and re-enabled which resolved the warning (vSphere HA initiated machine failover action in cluster {X} in datacenter {X}).

![ha_failover_warning](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/10/ha_failover_warning.png?resize=510%2C110)