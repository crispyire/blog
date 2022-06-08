---
id: 529
title: 'Microsoft PowerShell on Mac OS X'
date: '2016-09-08T21:56:21+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2016/09/513-revision-v1/'
permalink: '/?p=529'
---

![powershell-logo](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/09/powershell-logo.png?resize=300%2C203)I was very excited to hear that at VMworld US 2016, [Alan Renouf](https://twitter.com/alanrenouf) announced that PowerCLI is coming to Mac. No release date has been announced but meanwhile I wanted to see what I could do with Microsoft’s release of PowerShell for Mac.

It must be pointed out that, as Microsoft has released PowerShell as open source, support is going to be limited to non-existent from Microsoft. So tread with care!

Firstly, as PowerShell runs on .Net you will need to download and install [.NET Core for Mac](https://www.microsoft.com/net/core). There are some hoops to jump through as you need to use [Homebrew](https://github.com/Homebrew/) to get .Net to work on Mac OS X. The steps though are very clear on the Microsoft site.

Once you have installed the .Net Framework, you need to download and install PowerShell. This can be downloaded from the PowerShell project’s [Releases page](https://github.com/PowerShell/PowerShell/releases/) on GitHub.

![GitHub-Powershell](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/09/Screen-Shot-2016-09-08-at-20.35.55.png?resize=822%2C432)

Once the .PKG file is downloaded, you can install it…

![PowerShellInstall](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/09/PowerShellInstall.png?resize=650%2C300)

..and once installed, open up Terminal and install OpenSSL

```
brew install openssl
brew link openssl --force
```

and type

```
powershell
```

![PowerShellTerm](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/09/Screen-Shot-2016-09-08-at-20.45.06.png?resize=585%2C176)

Microsoft PowerShell is now installed, and working!

![PowerShellVersion](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/09/Screen-Shot-2016-09-08-at-21.13.47.png?resize=571%2C396)

You can now run your PowerShell .ps1 scripts that use the core modules without issue.