---
id: 2280
title: 'ESXi host loses connectivity to IBM storage'
date: '2019-11-25T15:12:23+00:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=2280'
permalink: /2019/11/esxi-host-loses-connectivity-to-ibm-storage/
ct_ignite_last_updated:
    - default
categories:
    - IBM
    - Storage
    - VMware
tags:
    - esxi
    - IBM
    - storage
    - vsphere
---

After a few incidents recently with clients using IBM storage, I thought I would write this quick post to remind people that ATS Heartbeats on IBM Storage arrays should be disabled to avoid possible outages due to the ESXi hosts losing connectivity to their datastores. The issue relates to VMware ESXi 6.5, 6.0 and 5.5 using VMFS 3 and VMFS 5 datastores

The issue only happens in certain conditions so I advise that you read and action <https://kb.vmware.com/s/article/2113956> and <https://www.ibm.com/support/pages/host-disconnects-using-vmware-vsphere-55-update-2-55-update-3-60-60-update-1-and-60-update-2>