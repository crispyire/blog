---
id: 835
title: 'PowerCLI on Mac OSX'
date: '2018-04-26T23:36:42+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2018/04/821-revision-v1/'
permalink: '/?p=835'
---

I’m a great fan of Microsoft PowerShell and VMware PowerCLI to making my life easier through automation. When Microsoft and VMware released PowerShell for Mac OSX / High Sierra and PowerCLI I jumped at the chance. There are a few things though once you have completed the setup that you are likely to want to do.

Firstly, the installation… There are some great blogs on installing [PowerShell Core](https://github.com/PowerShell/PowerShell) and [PowerCLI 10](https://blogs.vmware.com/PowerCLI/2018/02/powercli-10.html) so I will not cover that.

### Closing Terminal window

If you are ever faced with wanting to close either PowerShell or Terminal and the window does not close the window afterward this should help:

![Terminal Console - Not closing](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2018/04/Screen-Shot-2018-04-26-at-22.10.52.png?resize=300%2C210)

1. Open Terminal
2. Go to **Terminal** → **Preferences**
3. Select the **Settings** tab
4. Select your profile and choose the **Shell** tab
5. Set **When the shell exits** to **Close if the shell exited cleanly**

![Terminal - Options](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2018/04/Screen-Shot-2018-04-26-at-22.11.41.png?resize=300%2C270)

Once this is set, when you type ‘exit’ in the terminal console, the window will close too.

### Creating an Alias

I found that I could never remember that to load PowerCLI into Terminal once installed I need to type: `pwsh` so I wanted to change that to something I could remember like `Powershell`

1. Open Terminal
2. Type `vim ~/.bash_profile`
3. Type `i`
4. Type `alias powershell='pwsh'`or `alias powercli='pwsh'`
5. Type the Esc key, Type `:wq`
6. Type `source ~/.bash_profile` (This reloads the profile)

![Terminal - Alias](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2018/04/Screen-Shot-2018-04-26-at-22.28.38.png?resize=300%2C210)

Once the alias is set, you can now type `powershell` (Or whatever you changed it too) and PowerShell installed on your Mac will load.

### PowerShell Profiles

On a Mac, you can create a profile to load your custom settings each time. \[[I created a Windows version on a previous blog](http://www.baylyparker.com/2015/09/powershell-profiles-for-vmware-powercli/)\]

The profile location on Max OSX is:

`~/.config/powershell/profile.ps1`

```
<pre class="brush: powershell; title: ; notranslate" title="">
Set-Location ~/Dropbox/Scripts/ 
Clear-Host 
Write-Host 'Crispyland enabled'
```