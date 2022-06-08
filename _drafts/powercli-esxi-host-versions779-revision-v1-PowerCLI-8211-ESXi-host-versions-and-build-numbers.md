---
id: 786
title: 'PowerCLI &#8211; ESXi host versions and build numbers'
date: '2017-12-14T09:26:05+00:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2017/12/779-revision-v1/'
permalink: '/?p=786'
---

I’m currently in the process of upgrading a number of hosts in an environment. I needed to see the status of the host upgrading and patching process. Below is an easy way to get a list of ESXi host versions and build numbers via Powershell/PowerCLI.

```
<pre class="brush: powershell; title: ; notranslate" title="">

Get-View -ViewType HostSystem -Property Name,Config.Product | Format-Table Name, @{L='Host Version &amp; Build Version';E={$_.Config.Product.FullName}}

```

![BuildName1](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2017/12/BuildName1.png?resize=676%2C551)