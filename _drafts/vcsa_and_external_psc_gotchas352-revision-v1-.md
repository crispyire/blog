---
id: 353
date: '2016-02-03T12:03:56+00:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2016/02/352-revision-v1/'
permalink: '/?p=353'
---

When installing a new VMware data centre for a client this week, I took the design decision to go down the VMware vCenter Server Appliance (VCSA) route for their new ESXi 6 / vCenter 6 infrastructure. With vSphere 6 you have the new VMware Platform Services Controller (PSC); a replacement for vCenter Single Sign-On (SSO) and completes other security requirements such as licensing, certificate management and server reservation. Due to the multiple sites, across multiple continents and further planned expansion, an external PSC was built into the design.

The nature of using the new external PSC gave me a number of gotchas that I found as I built out the new vCenter. Here are a few I want to share with you:

1. **Make sure your DNS entries are setup correctly.**

The client had setup the DNS entries for the Hosts, VC and PSC as requested, but the Forward and Reverse DNS entries had different capitalisations. Make sure they are same case (I would advice lowercase all the time). Otherwise the install may fail.

2. **Make sure you install the Client Integration Plugin.**

The new install method for the VCSA is via a web UI from the ISO. This has great benefits of been able to install it from outside of the C#/Fat Client. My experience is that you have to browse the ISO file and run the VMware-ClientIntergrationPlugin-6.0.0.exe manually (and first) or you will not be able to start the install

3. **Ensure the client you are installing from has Java Runtime Environment (JRE)**

The install wizard is Java based and I found that due to security restrictions on the clients site, I could not use Internet Explorer. I completed the install with Chrome and on a machine that had Java JRE installed

4. **Make sure your VLAN on the VM Network is set correctly**

When the appliance installs it will ask which network/switch to add to. If you don’t first login to the host and set the ‘VM Network’ to the correct VLAN ID the installation will fail. During the install you will get the error message “Failed to start services. Firstboot Error.” The error relates to appliance not have network connectivity to Host, DNS and NTP servers.

5. **Make sure you only setup one NTP and DNS Server during the install**

There seems to be an issue with the install that if you specify multiple NTP and DNS servers the VCSA gets into a knot, resulting in failed install. The error I got was: ”Firstboot script execution error “. Use a single DNS and NTP server during the install and add any additional servers, once the install is complete.

6. **Make sure NTP servers are configured and Synced**

Ensure that Host and client on which you do the install from are in time sync. There is very little margin of drift allowed. Time drift will result in a failed install.

7. **Always use FQDN at all times.**

This is stated a number of times in the install wizard but once I had deployed the PSC (Which I used FQDN) and then tried to install the VC, I switched to using the IP address to connect the VC and the PSC. The result is that the certificates don’t match and not only does it result in a failed install of the VC but also breaks the PSC and that will need to be re-installed as well.