---
id: 319
title: 'PowerShell profiles for VMware PowerCLI'
date: '2018-04-26T23:27:21+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2015/11/293-autosave-v1/'
permalink: '/?p=319'
---

After reading and watching the video on Chris Wahl’s blog ([wahlnetwork.com](http://www.wahlnetwork.com)), I started to look into implementing PowerShell profiles in my life.

Though I have used PowerShell / PowerCLI for some time now, it has always been one liners to get a simple task done either because it can only be done that way (eg. Microsoft Exchange) or it was a repeat task.

As I have dealt with larger and larger VMware environments, it has become paramount that I embrace the world of automation and scripting or I would never realise half of what I set out to do. My scripts have got longer and more ambitious, but doing the hard work of setting up the environment so it works on any of my devices has always been a pain point.

To make my life easier, I took some of Chris’s post and made it my own… I created a profile.ps1 file in:

%USERPROFILE%\\Documents\\WindowsPowerShell\\![PowerShell Profile location](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/PSProfile.png?resize=803%2C234)

That reads:

```
<pre class="brush: powershell; title: ; notranslate" title="">
Set-ExecutionPolicy -ExecutionPolicy Bypass -Force -Scope CurrentUser
Set-Location C:\OneDrive\Scripts\
Clear-Host
Write-Host 'Crispyland enabled'
}
```

The first change was that I didn’t want to run the PowerShell as Administrator each time; so I set the Execution Policy in this fashion which seems to work so far in ‘normal’ mode.

I also wanted to point everything at a shared storage so I can run from home or work, and so pointed my scripts to my Microsoft OneDrive account.

![PowerShell Profile OneDrive Location](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/PSProfile-OneDrive.png?resize=803%2C234)

Wanting to run my PowerCLI scripts from PowerShell, I also used [Patrick Terlisten](http://www.vcloudnine.de/load-vmware-powercli-snap-in-automatically-in-powershell-ise/) blog one liner to automatically load the VMware PowerCLI modules into PowerShell.

I liked the little welcome message from Chris’s blog, so I also added my own version.

![PowerShell Loading](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/PSProfile-loading.png?resize=801%2C609)

So far so good… Happy scripting