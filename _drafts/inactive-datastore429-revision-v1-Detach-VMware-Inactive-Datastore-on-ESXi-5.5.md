---
id: 438
title: 'Detach VMware Inactive Datastore on ESXi 5.5'
date: '2016-05-19T13:50:39+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2016/05/429-revision-v1/'
permalink: '/?p=438'
---

While covering for a colleague at one of our clients, I noticed that one of the datastores was showing as inactive on one of the hosts (ESXi 5.5 Express Patch 10, 3568722)

![datastore_inactive](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/05/datastore_inactive.png?resize=150%2C65)

After rescanning the hosts, and then trying to unmount the datastore it had become clear that it was an issue with a single host as all the other hosts had successfully unmounted the LUN.

When I tried to remove it from the troublesome host, I got the error message:

<figure aria-describedby="caption-attachment-430" class="wp-caption alignnone" id="attachment_430" style="width: 300px">![datastore_error](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/05/datastore_error.png?resize=300%2C161)<figcaption class="wp-caption-text" id="caption-attachment-430">Operation failed, diagnostics report: Unable to query live VMFS state of volume.: No such file or directory</figcaption></figure>

After trying to unmount it via by [PowerCLI](https://www.vmware.com/support/developer/PowerCLI/PowerCLI41U1/html/Remove-Datastore.html)

![Datastore_PowerCLI](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/05/Datastore_PowerCLI.png?resize=300%2C175)

> Remove-Datastore -Datastore Datastore -VMHost 10.23.112.234 -Confirm:$false

I also tried via ESXCLI to stop start storage IO

> \# services.sh restart

and

> \# /etc/init.d/storageRM stop | start

..all was unsuccessful.

In the end, a host reboot resolved the issue. Sometimes, the simplest answer is the quickest answer.