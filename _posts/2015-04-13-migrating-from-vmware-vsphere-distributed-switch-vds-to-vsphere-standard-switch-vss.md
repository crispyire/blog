---
id: 111
title: 'Migrating from VMware vSphere Distributed Switch (vDS) to vSphere Standard Switch (vSS)'
date: '2015-04-13T10:20:31+01:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=111'
permalink: /2015/04/migrating-from-vmware-vsphere-distributed-switch-vds-to-vsphere-standard-switch-vss/
amazon-product-excerpt-hook-override:
    - '3'
amazon-product-content-hook-override:
    - '2'
amazon-product-newwindow:
    - '3'
amazon-product-content-location:
    - '1'
categories:
    - VMware
tags:
    - esxi
    - migrating
    - networking
    - vDS
    - vmware
    - 'vSphere Distributed Switch'
    - 'vSphere Standard Switch'
    - vSS
---

Over the last week, I have being migrating hundreds of hosts from one vSphere to another to facilitate an architectural redesigned provided to my client by VMware PSO.

As part of the migration, the hosts were going to be reimaged and new network settings applied to them. Each host had two physical NICs of which both were used in a VMware Distributed switch (One pNIC for VM Network/Management, the other dedicated for vMotion)

This posed a problem that I still needed to remain control of the hosts while I removed them from vSphere. For those reading this that has ever tried to remove a host from vSphere you will find there are some hoops you need to jump through:

- The host must be in maintenance mode, or you get an error like this:

![maintenance_mode](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/maintenance_mode.png?resize=400%2C116)

- The removal of the host will result in loss of access to VMs, Resource Pool, vApps that remain on the host as well as the admin/log data associated with the host.

![remove_host_warning](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/remove_host_warning.png?resize=400%2C237)

- The host must be removed from the VMware Distributed Switch (vDS) before attempted removal of the host from vSphere.

![error_vDS_removal](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/error_vDS_removal.png?resize=400%2C254)

The obvious solution if to remove the host from the vDS and then you can remove the host (Assuming you have placed it in maintenance mode as per the above error).

\[Home\] -&gt; \[Inventory\] -&gt; \[Networking\]

![remove_dVS](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/remove_dVS.png?resize=400%2C160)![reconfigure_vDS](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/reconfigure_vDS.png?resize=400%2C177)

Opps.. That didn’t work… If you read the error message it is complaining that a VMKernel is still in use. That results in the source of this blog; you can’t remove the Management VMKernel otherwise you lose management access and you can’t migrate it (yet) because all your physical NICs are in use. Therefore graceful removal of the hosts from vSphere is like Catch 22.

So here is what I came up with:

1. Evacuate the host of all VMs and vApps by placing the host into Maintenance Mode
2. Remove all the VMKernel from the vDS by selecting ones NOT used for management traffic by going to the \[Manage Virtual Adapters\] of the \[vSphere Distributed Switch\] section of \[Networking\] , selecting the non-management VMKernel and selecting ‘remove’. Once there is only the VMKernel port marked for Management Traffic left, select \[Close\] to exit the \[Manage Virtual Adapters\] window.![vmk1](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/vmk1.png?resize=400%2C200)
3. The next aim is to free up at least one physical uplink port, which is done via the \[Manage Physical Adapters\] option on the vDS. Select the uplink (In this case ‘vmnic1’) and selecting \[remove\], accept the warning, and select \[ok\] to exit the \[Manage Physical Adapters\] window.![pNICs](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/pNICs.png?resize=400%2C134)
4. Select the \[vSphere Standard Switch\] window, select \[Add Networking\], select \[Virtual Machine\] and \[Next\]. You should select the pNIC that you just removed from the vDS and then \[Next\]![vSS_add](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/vSS_add.png?resize=400%2C148)
5. At the next window, give the network and name, VLAN (If required in your network). The VLAN ID is not important as the aim is to create a new Standard Switch (vSS) not a working virtual machine network for live traffic to go down. Once this is completed, select \[Next\], read the summary and select \[Finish\]![vSS_add2](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/vSS_add2.png?resize=400%2C161)
6. The \[Networking\] screen will look similar to this![vSS](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/vSS.png?resize=400%2C132)
7. Return to the \[vSphere Distributed Switch\] window, and select \[Manage Virtual Adapters\]. Select the remaining VMKernel (The one marked as ‘Management Traffic: Enabled’) and select \[Migrate\]![dVS_Migrate](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/dVS_Migrate.png?resize=400%2C115)
8. Follow the prompts, selecting the vSS switch you have just created, filling in a network label and a VLAN ID (This time it is important that this is correct!) and select \[Finish\] ![vDS_to_vSS_Migrate](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/vDS_to_vSS_Migrate.png?resize=400%2C137)
9. When the migration has completed, you can check that networking is now host-centric and not vSphere-centric by selecting \[vSphere Standard Switch\] and you can now see that the management traffic VMKernel has been migrated.![vSS_summary](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/vSS_summary.png?resize=400%2C204)
10. The host can now be removed from the vDS via \[Home\] -&gt; \[Inventory\] -&gt; \[Networking\], and selecting the vDS, and the \[Hosts\] tab. Find the host you want to remove, right click on the host and select \[Remove from vSphere Distributed Switch\]
11. When this has completed, return to the hosts view \[Home\] -&gt; \[Inventory\] -&gt; \[Hosts and Clusters\], right click on the host and select \[Remove\], read the warnings and select \[Yes\]![remove_host_warning](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/04/remove_host_warning.png?resize=400%2C237)
12. The host will now be successful removed from vSphere and its management influence.

The host has now been removed from the vSphere Distributed Switch and vSphere. The host is now independent of vSphere management and be reconfigured, reimaged or decommissioned as required.