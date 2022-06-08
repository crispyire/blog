---
id: 322
title: 'PowerCLI &#8211; Retrieving VM Disk Controller Types'
date: '2015-11-02T16:35:17+00:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2015/11/307-revision-v1/'
permalink: '/?p=322'
---

I have recently been auditing a client’s cloud infrastructure to looking for configuration difference between vCenters/PODs.

Here is a helpful little PowerCLI script to review all the Virtual SCSI Controllers for VM’s in your environment.

```
<pre class="brush: powershell; title: ; notranslate" title="">
# Retrieving VM Disk Controller Types
# V1.0 by Christian Parker

# Set Variables
$POD = &quot;vcenter.domain.local&quot;
$Cluster1 = &quot;Management&quot;

# Connect to vSphere
Write-Host &quot;Connecting to&quot; $POD
Connect-VIServer $POD | Out-Null

# Retrieve VM Disk Controller Types
Write-Host &quot;Collecting Virtual SCSI Controllers for&quot; $POD
Get-VM -Location $Cluster1 | Select Name,@{N=&quot;Cluster&quot;;E={Get-Cluster -VM $_}},@{N=&quot;Controller Type&quot;;E={Get-ScsiController -VM $_ | Select -ExpandProperty Type}} | Export-Csv C:\Temp\vSCSI_Type.csv -NoTypeInformation

# Disconnect from vSphere
Disconnect-VIServer $POD -Force -confirm:$false
```