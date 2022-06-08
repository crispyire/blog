---
id: 626
title: 'Duplicate vNIC on Windows VMs after P2V or restore from backup'
date: '2016-11-15T15:47:39+00:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2016/11/615-revision-v1/'
permalink: '/?p=626'
---

At present, I’m involved in a project for a client to migrate virtual machines (VM) from one data centre in Ireland to another in Continental Europe.

I encountered an issue after backing up the VM and restoring it in the new DC with NetBackup.

Once the VM was restored, registered in vCenter and powered on, I needed to re-IP the VM. I saw that I had no IP connectivity (Couldn’t ping, RDP, etc). After some initial troubleshooting I found that the Windows Server 2008 R2 Network and Sharing Center was reporting two active connections (Though the VM had one NIC) and to confirm the one NIC; Network Connections only showed a single network adapter. Both Windows Device Manager and VM settings show a single NIC device available.

![single_nic](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/11/Single_NIC.png?resize=300%2C128)

but the default route was showing another connection:

![dw_bug](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/11/DW_Bug.png?resize=522%2C43)

I had come across this issue before in another context, and the following resolved the issue:

From the command prompt run:

```
<pre class="brush: plain; title: ; notranslate" title="">set devmgr_show_nonpresent_devices=1
```

![devmgr](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/11/devmgr.png?resize=607%2C106)

Then open Device Manager and select:

```
<pre class="brush: plain; title: ; notranslate" title="">View--&gt;Show hidden devices
```

![hidden-devices](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/11/Hidden-Devices.png?resize=273%2C176)  
And then select Network adaptors and delete **ALL** Ethernet Adapters including any greyed out.

after the delete, select:

```
<pre class="brush: plain; title: ; notranslate" title="">Action--&gt;Scan for hardware changes
```

![hardware-changes](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/11/hardware-changes.png?resize=300%2C136)

You should be left with a single vNIC (Or as many vNICs as you have assigned to the VM)  
In the Command Prompt issue the following command to set the default IP/route.

```
<pre class="brush: plain; title: ; notranslate" title=""> netsh interface ip set address &quot;Local Area Connection&quot; static IP_Address Subnet_Mask Default_Gateway 
```

So, for example:

```
<pre class="brush: plain; title: ; notranslate" title="">netsh interface ip set address &quot;Local Area Connection 2&quot; 10.100.3.17 255.255.255.0 10.100.3.1
```

Re-IP any other NICs you have assigned to the VM and connectivity should now be restored.