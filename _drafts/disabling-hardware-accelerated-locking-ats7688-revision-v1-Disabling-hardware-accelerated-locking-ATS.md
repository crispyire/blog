---
id: 7692
title: 'Disabling hardware accelerated locking (ATS)'
date: '2021-03-04T15:30:00+00:00'
author: 'Christian Parker'
layout: revision
guid: 'https://www.baylyparker.com/2021/03/7688-revision-v1/'
permalink: '/?p=7692'
---

Due to changes in vSphere 6.x, SCSI reads and writes with the VMware ESXi kernel handling validation, host disconnects can occur if delays of 8 seconds or longer are experienced in completing individual heartbeat I/Os.

For VMFS5/VMFS6 datastores, ATS heartbeat setting is **on** by default. The default behaviour is ok for VSAN clusters but should be changed to **off** for other storage options.

Though you can change this via host CLI, it means logging onto each host in your enviornment to make the changes. If you want to do this option here is the command:

*esxcli system settings advanced set -i 0 -o /VMFS3/UseATSForHBOnVMFS5*

but I would recommend using the PowerCLI command as you can target multiple hosts at the same time. The example below firstly shows the ATS status for all hosts in a vSphere cluster:

*Get-Cluster -Name Lab-Cluster | Get-VMHost | Get-AdvancedSetting -Name VMFS3.UseATSForHBOnVMFS5 | ft Entity, Name, Value -AutoSize*

<figure class="wp-block-image size-large">[![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2021/03/Screenshot-2021-03-04-at-14.15.19.png?resize=629%2C176&ssl=1)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2021/03/Screenshot-2021-03-04-at-14.15.19.png?ssl=1)</figure>(Please note that the default is 1 and not 0 but I had already made the changes to this environment)

..and then making the changes to disable ATS Heartbeating (Value 0) for the same cluster:

*Get-Cluster -Name Lab-Cluster | Get-VMHost | Get-AdvancedSetting -Name VMFS3.UseATSForHBOnVMFS5 | Set-AdvancedSetting -Value 0 -Confirm:$false*

<figure class="wp-block-image size-large">[![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2021/03/Screenshot-2021-03-04-at-14.16.02-1.png?resize=635%2C156&ssl=1)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2021/03/Screenshot-2021-03-04-at-14.16.02-1.png?ssl=1)</figure>