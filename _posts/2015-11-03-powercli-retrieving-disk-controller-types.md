---
id: 307
title: 'PowerCLI &#8211; Retrieving VM Disk Controller Types'
date: '2015-11-03T11:00:26+00:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=307'
permalink: /2015/11/powercli-retrieving-disk-controller-types/
amazon-product-content-location:
    - '1'
amazon-product-excerpt-hook-override:
    - '3'
amazon-product-content-hook-override:
    - '2'
amazon-product-newwindow:
    - '3'
categories:
    - VMware
tags:
    - automation
    - PowerCLI
    - PowerShell
    - vsphere
---

I have recently been auditing a client’s cloud infrastructure to looking for configuration difference between vCenters/PODs.

Here is a helpful little PowerCLI script to review all the Virtual SCSI Controllers for VM’s in your environment.

```
<pre class="brush: powershell; title: ; notranslate" title="">
# Retrieving VM Disk Controller Types
# V1.0 by Christian Parker

# Set Variables
$POD = "vcenter.domain.local"
$Cluster1 = "Management"

# Connect to vSphere
Write-Host "Connecting to" $POD
Connect-VIServer $POD | Out-Null

# Retrieve VM Disk Controller Types
Write-Host "Collecting Virtual SCSI Controllers for" $POD
Get-VM -Location $Cluster1 | Select Name,@{N="Cluster";E={Get-Cluster -VM $_}},@{N="Controller Type";E={Get-ScsiController -VM $_ | Select -ExpandProperty Type}} | Export-Csv C:\Temp\vSCSI_Type.csv -NoTypeInformation

# Disconnect from vSphere
Disconnect-VIServer $POD -Force -confirm:$false
```