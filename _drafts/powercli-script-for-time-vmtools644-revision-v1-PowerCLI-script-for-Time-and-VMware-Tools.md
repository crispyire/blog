---
id: 650
title: 'PowerCLI script for Time and VMware Tools'
date: '2017-01-04T14:04:37+00:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2017/01/644-revision-v1/'
permalink: '/?p=650'
---

One of my clients I love spending time with have a constant battle to keep VMware Tools up-to-date and keeping the VMs time in sync. I sadly, don’t get to spend as much time with them as I would like but this week and next week I’m onsite and wanted to see how I can help them out.

Though there are some great blog posts about setting the advanced features of VMware Tools within a VM but I wanted to audit what was currently set:

![VM Advanced Options](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/01/VM-Advanced-Options.png?resize=300%2C267)

…and created the following two PowerCLI one liners to give me that output to review:

The first enables VMware Tools upgrade on power cycle and is equivalent to the GUI tick box “**Check and Upgrade Tools during power cycling**”

```
<pre class="brush: powershell; title: ; notranslate" title="">
Get-View -ViewType virtualmachine | Select name,@{N='ToolsConfigInfo';E={$_.Config.Tools.syncTimeWithHost } } | Export-Csv &quot;c:\temp\VMToolsStatus.csv&quot; -NoTypeInformation
```

The second enables VM syncing its time with the host and is equivalent to the GUI tick box “**Synchronize guest time with the host**”

```
<pre class="brush: powershell; title: ; notranslate" title="">
Get-View -ViewType virtualmachine | select name,@{N='ToolsUpgradePolicy';E={$_.Config.Tools.ToolsUpgradePolicy } } | Export-Csv &quot;c:\temp\TimeSyncStatus.csv&quot; -NoTypeInformation
```

These two one liner were based on the setting commands available to automatically upgrade VMware Tools on the [VMware KB 1010048](https://kb.vmware.com/kb/1010048).