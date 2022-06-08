---
id: 655
title: 'PowerCLI script for Time and VMTools'
date: '2017-01-04T14:25:14+00:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2017/01/644-revision-v1/'
permalink: '/?p=655'
---

One of my clients I love spending time with have a constant battle to keep VMware Tools (VMTools) up-to-date and keeping the VMs time in sync. I sadly, don’t get to spend as much time with them as I would like but this week and next week I’m onsite and wanted to see how I can help them out.

Though there are some great blog posts about setting the advanced features of VMware Tools within a VM but I wanted to audit what was currently set:

![VM Advanced Options (VMTools and Time sync)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/01/VM-Advanced-Options.png?resize=300%2C267 "VM Advanced Options (VMTools and Time sync)")

…and created the following two PowerCLI one liners to give me that output to review:

The first gets a list of VMs that have their VMware Tools setting to upgrade on power cycle, and is equivalent to the GUI tick box “**Check and Upgrade Tools during power cycling**”

```
<pre class="brush: powershell; title: ; notranslate" title="">
Get-View -ViewType virtualmachine | Select name,@{N='ToolsConfigInfo';E={$_.Config.Tools.syncTimeWithHost } } | Export-Csv &quot;c:\temp\VMToolsStatus.csv&quot; -NoTypeInformation
```

The second audit, gives a list of VMs that are syncing its time with the host and is equivalent to the GUI tick box “**Synchronize guest time with the host**“. Though it would be best practice to have the VM get its time from a NTP source (Ideally the same as the host), in some cases this isn’t possible due to OS restrictions, and therefore this is a good choice.

```
<pre class="brush: powershell; title: ; notranslate" title="">
Get-View -ViewType virtualmachine | select name,@{N='ToolsUpgradePolicy';E={$_.Config.Tools.ToolsUpgradePolicy } } | Export-Csv &quot;c:\temp\TimeSyncStatus.csv&quot; -NoTypeInformation
```

These two one liner were based on the setting commands available to automatically upgrade VMware Tools on the [VMware KB 1010048](https://kb.vmware.com/kb/1010048).

So the code from the KB article can be used to set the automatic upgrading of the VMware Tools on power cycle:

```
<pre class="brush: powershell; title: ; notranslate" title="">
Foreach ($v in (get-vm)) {
$vm = $v | Get-View
$vmConfigSpec = New-Object VMware.Vim.VirtualMachineConfigSpec
$vmConfigSpec.Tools = New-Object VMware.Vim.ToolsConfigInfo
$vmConfigSpec.Tools.ToolsUpgradePolicy = &quot;UpgradeAtPowerCycle&quot;
$vm.ReconfigVM($vmConfigSpec)
}
```

…and with some minor alterations can be used to set the time syncing option:

```
<pre class="brush: powershell; title: ; notranslate" title="">
Foreach ($v in (get-vm)) {
$vm = $v | Get-View
$vmConfigSpec = New-Object VMware.Vim.VirtualMachineConfigSpec
$vmConfigSpec.Tools = New-Object VMware.Vim.ToolsConfigInfo
$vmConfigSpec.Tools.syncTimeWithHost = $true
$vm.ReconfigVM($vmConfigSpec)
}
```

Be warned this will set the option on all VMs within your vCenter. If you want to only do a number of VMs either do manually or alter the script