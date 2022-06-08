---
id: 453
title: 'Migration from vCenter 5.1 to 6.0 &#8211; Issues with EVC Mode'
date: '2016-06-21T11:34:59+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2016/06/446-revision-v1/'
permalink: '/?p=453'
---

This week I have being migrating VMs from a vCenter 5.1 environment to a new vCenter 6.0 for a client

As the client must have as near to zero downtime as possible, due to being in the financial sector, we opted to use a shuttle host (ESXi5.1, 1065491). This was to facilitate the live transfer of VMs to their new homes in vSphere 6.0.

There was a number of gotchas during the process the first being the migration from different EVC modes as the VMs moved across different CPU hardwares.

The old 5.1 clusters in the old vCenter were running EVC Mode at *Intel “Nahalem” Generation* and the new vCenter 6.0 clusters were running *Intel “Haswell” Generation*. (Please note that EVC mode under Nahalem is not supported in vCenter 6.0!)

This wouldn’t normally be an issue with a cold migration as the CPUID masking would change at power on, but as these VMs are going to be moved without power down/power cycling. The result is VMs in the same cluster holding their EVC mode from their previous hosts/clusters.

![EVCMode-VMs](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/06/EVCMode-VMs.png?resize=461%2C162)

This didn’t seem to effect day-to-day operations of the VMs but was far from ideal and a series of maintenance windows has been schduled to power down and reboot these VMs in the very near future. It must be noted that power cycling/rebooting the VMs does not change the EVC mode. The VM must be powered down and powered back on for the new EVC mode to be applied.

The biggest concern is what would happen if the VMs with the Nahalem EVC mode were rebooted. The answer is: It depends on how it was vMotioned!

Those VMs vMotioned directly onto a host would fail to power on after either a cold start or reboot. Those VMs vMotioned to a cluster would power on without issue.

![vMotion-Cluster](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/06/vMotion-Cluster.png?resize=842%2C270)