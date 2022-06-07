---
id: 446
title: 'Migration to vCenter 6.0 &#8211; Issues with EVC Mode'
date: '2016-06-22T11:00:09+01:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=446'
permalink: /2016/06/migration-vcenter-5-1-6-0-issues-evc-mode/
categories:
    - VMware
tags:
    - '5.1'
    - '6.0'
    - esxi
    - Migration
    - vCenter
    - vmware
    - vsphere
---

This week I have being migrating VMs from a vCenter 5.1 environment to a new vCenter 6.0 for a client

As the client must have as near to zero downtime as possible, due to being in the financial sector, we opted to use a shuttle host (ESXi5.1, 1065491). This was to facilitate the live transfer of VMs to their new homes in vSphere 6.0.

There was a number of gotchas during the process the first being the migration from different [EVC modes](https://kb.vmware.com/kb/1005764) as the VMs moved across different CPU hardwares.

The old 5.1 clusters in the old vCenter were running EVC Mode at *Intel “Nahalem” Generation* and the new vCenter 6.0 clusters were running *Intel “Haswell” Generation*. (Please note that EVC mode under Nahalem is [not supported in vCenter 6.0](https://kb.vmware.com/kb/1003212)!)

This wouldn’t normally be an issue with a cold migration as the CPUID masking would change at power on, but as these VMs are going to be moved without power down/power cycling. The result is VMs in the same cluster holding their EVC mode from their previous hosts/clusters.

![EVCMode-VMs](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/06/EVCMode-VMs.png?resize=461%2C162)

This didn’t seem to effect day-to-day operations of the VMs but was far from ideal and a series of maintenance windows has been schduled to power down and reboot these VMs in the very near future. It must be noted that power cycling/rebooting the VMs does not change the EVC mode. The VM must be powered down and powered back on for the new EVC mode to be applied.

The biggest concern is what would happen if the VMs with the Nahalem EVC mode were rebooted. The answer is: It depends on how it was vMotioned!

Those VMs vMotioned directly onto a host would fail to power on after either a cold start or reboot. Those VMs vMotioned to a cluster would power on without issue.

![vMotion-Cluster](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/06/vMotion-Cluster.png?resize=842%2C270)

So if you are able to either remove EVC mode from the VMs before or after the migration that would be ideal. If you can’t straight away you should migrate to the cluster and not the host as in the event of a reboot (Planned or not) you are most likely to find your VM will not power on again. This issue will continue until either EVC Mode is removed from the new cluster or as I did, I migrated it back to the original host. I then migrated it back to the cluster instead of directly to the host. The result was the VM then powered on without issue.