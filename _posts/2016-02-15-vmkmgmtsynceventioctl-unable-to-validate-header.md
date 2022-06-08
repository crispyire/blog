---
id: 370
title: 'VmkMgmtSyncEventIoctl &#8211; unable to validate header'
date: '2016-02-15T11:01:47+00:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=370'
permalink: /2016/02/vmkmgmtsynceventioctl-unable-to-validate-header/
categories:
    - Storage
    - VMware
tags:
    - Emulex
    - esxi
    - HBA
    - vmware
---

While revisiting an old client’s site I reviewed the logs and noticed that there was a high instance of the error message in vmkernel.log:

```
WARNING: VmkMgmtSyncEventIoctl - unable to validate header
```

![VmkMgmtSyncEventIoctl - unable to validate header](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/02/Emulex-error.jpg?resize=808%2C67)

..and a number of instances where the HBAs had becoming unresponsive during periods of high I/O.

After some research, it seemed that other people had similar issues but normally on HP hardware. During my search, I came across and reviewed <http://kb.vmware.com/kb/2086025>

The article makes no mention of server vendor but solely about the Emulex HBAs. As the client has IBM X440 on ESXi5.5 and most importantly have Emulex HBAs, this was hopefully the fix.

I initially downloaded the driver from the [VMware website](https://my.vmware.com/web/vmware/details?downloadGroup=DT-ESXI55-EMULEX-LPFC-10234018&productId=353) and tried to install it via VMware Update Manager (VUM), but this failed to add to the repository on multiple occasions, so I completed a CLI installation via:

![Emulex Update](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2016/02/Emulex.jpg?resize=666%2C169)

…after the reboot there is no instance of the error reoccurring.