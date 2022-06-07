---
id: 709
title: 'PowerCLI &#8211; Finding host bus adapter (HBA) World Wide Name (WWN)'
date: '2017-06-06T10:06:15+01:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=709'
permalink: /2017/06/powercli-finding-host-hba-wwn/
categories:
    - Scripting
    - VMware
tags:
    - automation
    - PowerCLI
    - PowerShell
    - vmware
---

While trying to get the World Wide Name (WWN) for a host that I needed zoning by the storage team, I ran into an issue working remotely that I couldn’t get at the webclient or the c# client.

PoweCLI to the rescue:

```
<pre class="brush: powershell; title: ; notranslate" title="">
Get-VMHostHBA -Type FibreChannel | Select VMHost,Device,@{N="WWN";E={"{0:X}" -f $_.PortWorldWideName}}
```

![WWN](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/06/wwn.png?resize=1408%2C155)

More information about ‘Get-VMHostHBA’ from [vSphere PowerCLI Cmdlets Reference](https://www.vmware.com/support/developer/PowerCLI/PowerCLI41U1/html/Get-VMHostHba.html)