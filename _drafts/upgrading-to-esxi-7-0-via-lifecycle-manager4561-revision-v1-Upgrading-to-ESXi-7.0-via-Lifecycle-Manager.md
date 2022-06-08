---
id: 4565
title: 'Upgrading to ESXi 7.0 via Lifecycle Manager'
date: '2020-05-27T23:44:01+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2020/05/4561-revision-v1/'
permalink: '/?p=4565'
---

I found an error when upgrading my ESXi 6.7 hosts to 7.0 today. The upgrade via Lifecycle Manager (AKA Update Manager (VUM)) shows as ‘incompatible’ when the host is remediated (upgraded). I have uncounted this before in previous upgrades but I have not seen documented in vSphere 7.0 yet.

<figure class="wp-block-image size-large">![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/Screenshot-2020-05-27-at-22.47.25.png?fit=1024%2C314)</figure>This is due to a ESXi 6.7 vSphere Installation Bundle (VIB) not being compatible with ESXi 7.0 but can be easily fixed.

Firstly, enable the SSH service on the affected host, and then SSH to that host that needs remediating. Search for the VIB that is creating the incompatibility which in this example is a network USB driver. Use the keywords in the details provided in Lifecycle Manager to search for the VIB in the CLI.

<figure class="wp-block-image size-large">![](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/Screenshot-2020-05-27-at-22.52.02.png?fit=1024%2C149)</figure>Once the exact name of the VIB is located, it can be removed with the following command:

<figure class="wp-block-image size-large">![](https://i1.wp.com/www.baylyparker.com/wp-content/uploads/2020/05/Screenshot-2020-05-27-at-22.51.31.png?fit=1024%2C224)</figure>Please note you may need to reboot the host before the VIB is fully removed.

The host can be upgraded to the latest version (vSphere 7.0)

Details from previous versions can be found at <https://kb.vmware.com/s/article/2140962>