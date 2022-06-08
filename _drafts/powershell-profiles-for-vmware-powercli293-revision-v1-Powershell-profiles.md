---
id: 297
title: 'Powershell profiles'
date: '2015-09-17T14:56:42+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2015/09/293-revision-v1/'
permalink: '/?p=297'
---

After reading and watching the video on Chris Wahl’s blog ([wahlnetwork.com](http://www.wahlnetwork.com)), I started to look into implementing Powershell profiles in my life.

Though I have used PowerShell / PowerCLI for some time now, it has always been one liners to get a simple task done either because it can only be done that way (eg. Microsoft Exchange) or it was a repeat task.

As I have dealt with larger and larger VMware environments, it has become paramount that I embrace the world of automation and scripting or I would never realise half of what I set out to do. My script have got long and more ambitious, but doing the hard work of setting up the environment so it works on any of my devices has always been a pain point.

To make my life easier, I took some of Chris’s post and made it my own… I created a profile.ps1 file in:

%USERPROFILE%\\Documents\\WindowsPowerShell\\[![PowerShell Profile location](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/PSProfile.png?resize=300%2C87)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/PSProfile.png)

That reads:

```
Set-ExecutionPolicy -ExecutionPolicy Bypass -Force -Scope CurrentUser
```

```
Set-Location C:\OneDrive\Scripts\
```

```
if ($psISE)
```

```
{
```

```
Add-PsSnapin VMware.VimAutomation.Core -ea 'SilentlyContinue'
```

```
Clear-Host
```

```
Write-Host 'Crispyland enabled'
```

```
}
```

The first change was that I didn’t want to run the PowerShell as Administrator each time; so I set the Execution Policy in this fashion which seems to work so far in ‘normal’ mode.

I also wanted to point everything at a shared storage so I can run from home or work, and so pointed my scripts to my Microsoft OneDrive account.

[![PowerShell Profile OneDrive Location](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/PSProfile-OneDrive.png?resize=300%2C87)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/PSProfile-OneDrive.png)

Wanting to run my PowerCLI scripts from PowerShell, I also used [Patrick Terlisten](http://www.vcloudnine.de/load-vmware-powercli-snap-in-automatically-in-powershell-ise/) blog one liner to automatically load the VMware PowerCLI modules into PowerShell.

I liked the little welcome message from Chris’s blog, so I also added my own version.

[![PowerShell Loading](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/PSProfile-loading.png?resize=300%2C228)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/PSProfile-loading.png)

So far so good… Happy scripting