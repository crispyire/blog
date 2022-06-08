---
id: 4569
title: 'PowerCLI &#8211; vCenter Snapshot Report'
date: '2020-05-28T10:32:48+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2020/05/4550-revision-v1/'
permalink: '/?p=4569'
---

A number of my clients have asked for a simple report that can be run against their vCenter to report on VM Snapshots.

Though I would recommend that they use vRealize Operations (vROPS), not every environment has it installed. I have compiled from a number of scripts and forums to tailor the following script. I have taken inspiration from a few of [Luc Dekens](https://twitter.com/LucD22)‘s scripts. I have tested this on vSphere 6.7 and 7.0

```
<pre class="wp-block-preformatted">#Variables
$vCenter = "vcsa01.baylyparker.local"
$vCenterUser = "baylyparker\ServiceAccount1"
$vCenterPass = "VMware1!"
$SMTPServer = "smtp.baylyparker.local"
$To = "admin@baylyparker.local"
$From = "vcsa01@baylyparker.local"

#HTML formatting
$style = "BODY{font-family: Arial; font-size: 10pt;}" $style = $style + "TABLE{border: 1px solid red; border-collapse: collapse;}" $style = $style + "TH{border: 1px solid; background-color: #4CAF50; color: white; padding: 5px; }" $style = $style + "TD{border: 1px solid; padding: 5px; }" $style = $style + "
"
$date = Get-Date -Format "dddd dd/MM/yyyy HH:mm K"

#Connect to vCenter"
CLS
Write-Host "Connecting to $vCenter" -ForegroundColor Blue
Connect-VIServer -Server $vCenter -User $vCenterUser -Password $vCenterPass -Force | Out-Null
Write-Host " Connected to $vCenter" -ForegroundColor Green

#Get list of VMs with snapshots
Write-Host "Generating VM snapshot report" -ForegroundColor Blue
$SnapshotReport = Get-vm | Get-Snapshot | Select-Object VM,Description,PowerState,SizeGB | Sort-Object SizeGB | ConvertTo-Html -Head $style | Out-String
Write-Host " Completed" -ForegroundColor Green

#Sending email report
Write-Host "Sending VM snapshot report" -ForegroundColor Blue
$htmlbody = @"
$SnapshotReport
"@
Send-MailMessage -smtpserver $SMTPServer -From $From -To $To -Subject "Snapshot Email Report for $Date" -BodyAsHtml -Body $htmlbody
Write-Host " Completed" -ForegroundColor Green

#Disconnecting vCenter
Disconnect-VIServer -Server $vCenter -Force -Confirm:$false
Write-Host "Disconnecting to $vCenter" -ForegroundColor Blue
```

The important part is that the variables are changed to match your environment and that the email server is configured to accept unauthenticated emails from where the script is being run from.

<figure class="wp-block-image size-large">![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/Screenshot-2020-05-27-at-15.53.26-1.png?resize=818%2C207)</figure>The script I suggest is run from a location that you can schedule the script to run on a regular basis (eg. Task Scheduler for Microsoft Windows Server)

```
<pre class="wp-block-preformatted"><strong>Please note that this sample script uses plain text passwords. Using plain text in a production environment is not recommended. You can adapt the script to encrypt the passwords which is not covered in this post.</strong>
```