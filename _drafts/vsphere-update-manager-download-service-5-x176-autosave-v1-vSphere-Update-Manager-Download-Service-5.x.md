---
id: 198
title: 'vSphere Update Manager Download Service 5.x'
date: '2015-08-10T09:41:57+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2015/08/176-autosave-v1/'
permalink: '/?p=198'
---

In my quest to create some standardisation at my clients site, I got stuck into the patching of 800 VMware hosts across 8 different vCenter instances. I was faced with hosts with ESXi5.0, ESXi5.1 and ESXi5.5 (And soon ESXi6.x), so standardisation is paramount.

Though I found the majority of hosts were (As expected) patched to the same level within a cluster, there was notable differences in patching levels between clusters and vCenters. Security and update patches are something of balancing act between fixing known (and sometimes unknown) issues and the risk the the update itself can introduce new issues into the environment.

My first task was to set a baseline to patches/fixes/updates that are ‘known good’ and apply that baseline across the Update Managers in the different vCenters. A recent vCenter had been commissioned and the latest updates from VMware and the latest drives/updates from the hardware vendor had been applied. No issues had been detected with the updates so this baseline deemed as the ‘known good’ and that baseline was rolled onto the other vCenters. Those baselines were then applied (Attached to) to the Datacenter level of vCenter.

As the vCenters had been architectured to be silos and independent on a compute level, so the same process had been applied to vCenter Update Manager (VUM). Though VUM itself has a one-to-one relationship with vCenter the patch repository does not have to be. The additional benefits of reducing complexity in scale was also realised by the single repository .

The installation of vSphere Update Manager Download Service (UMDS) is a simple enough process (Installed in 5.5 from the vCenter Server CD, open the folder ‘umds’, and run ‘VMware-UMDS’) and follow the prompts. As the load is low on the VM, I decided to use the builtin SQL database but if you have an external SQL server already setup you may wish to use that instead.

UMDS doesn’t have a GUI and is controlled via the Windows Command Line.

The following commands were issued to setup UMDS:

```
<strong>MD E:\inetpub\wwwroot\UMDS</strong>
```

```
<strong>vmware-umds -S --patch-store E:\inetpub\wwwroot\UMDS
<img alt="UMDS" class="aligncenter size-full wp-image-179" data-recalc-dims="1" height="128" loading="lazy" sizes="(max-width: 992px) 100vw, 992px" src="https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-3.png?resize=992%2C128" srcset="https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-3.png?w=992 992w, https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-3.png?resize=300%2C39 300w" width="992"></img>
</strong>
```

This changes the patch repository directory\\drive. I noticed that that in my Windows Server 2012R2 that when I tried using the root (eg. (e:\\) it reports all ok, but I got error message when downloading, so I changed it to a sub-directory and everything worked fine.

```
v<strong>mware-umds -S --enable-host --enable-va
<img alt="UMDS Screenshot" class="aligncenter wp-image-180 size-full" data-recalc-dims="1" height="96" loading="lazy" sizes="(max-width: 1000px) 100vw, 1000px" src="https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-1.png?resize=1005%2C96" srcset="https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-1.png?w=1005 1005w, https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-1.png?resize=300%2C29 300w" width="1005"></img>
</strong>
```

```
<strong>vmware-umds -S -d esx-4.0.0 embeddedEsx-4.0.0 esx-4.1.0 embeddedEsx-4.1.0
<img alt="UMDS" class="aligncenter size-full wp-image-181" data-recalc-dims="1" height="144" loading="lazy" sizes="(max-width: 993px) 100vw, 993px" src="https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-2.png?resize=993%2C144" srcset="https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-2.png?w=993 993w, https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-2.png?resize=300%2C44 300w" width="993"></img>
</strong>
```

This setup would download of all ESXi 5.x updates, and to disable downloading of ESX 4.x and ESXi 4.x host updates.

```
<strong>vmware-umds -S --add-url http://vibsdepot.hp.com/hpq/jun2015/index.xml --url-type HOST</strong>
```

As the setup was for HP blade servers, I also added in 3rd-party download location for drives/updates from HP.

You can confirm the setup via:

```
<strong>vmware-umds -G
<a href="https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-51.png"><img alt="UMDS" class="aligncenter wp-image-195 size-full" data-recalc-dims="1" height="319" loading="lazy" sizes="(max-width: 946px) 100vw, 946px" src="https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-51.png?resize=946%2C319" srcset="https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-51.png?w=946 946w, https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-51.png?resize=300%2C101 300w" width="946"></img></a>
</strong>
```

When you are ready, you can start the download of the patches/updates by issuing the command:

```
<strong>vmware-umds -D</strong>
```

You can also run the command

```
<strong>vmware-umds -R --start-time 2010-11-01T00:00:00 --end-time 2010-11-30T23:59:59</strong>
```

…which redownloads the patches between set times to reduce load during core business hours.

When it completes the download process (Which could take some time), the output should look something like this:

![UMDS](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/umds-4-e1438932460641.png?resize=675%2C128)

After the download process has completed, you can setup URL for easy access to the repository. This is achieved via installing IIS on your UMDS server. Firstly add the role to the server and configure as such:

1. Change the physical path of the Default Web Site to to location of your repository.
2. Add .vib, .sig, and .xml as allowed MIME types for the Web server. 
    - From Server Manager, load Internet Information Services (IIS) Manager.
    - In the Internet Information Services (IIS) Manager window, select Connections &gt; \[Computer Name\] &gt; Sites &gt; Default Web Site.
    - Click the UMDS folder where you exported the patch data and select MIME Types
    - Click Add and add the new MIME types. In the Extension text field, enter .vib, .sig, and .xml. Enter one file extension for each MIME type entry. In the MIME Type field, enter application/octet-stream for .vib and .sig. For .xml, enter text/xml in the MIME Type field.
3. Confirm that permissions are set to Anonymous Authentication
4. Restart the Internet Information Services (IIS) service (In the Internet Information Services (IIS) Manager window, select Connections &gt; \[Computer Name\] &gt; Actions &gt; Restart
5. You can now use the UMDS via URL
6. Connect the vSphere Client to a vCenter Server
7. Select Home &gt; Solutions and Applications &gt; Update Manager
8. Click the Configuration tab in the Update Manager Administration view.
9. Select the Use a shared repository radio button.
10. Enter the URL of the folder on the Web server which you have just created eg. http://ip\_address\_or\_hostname/UMDS
11. Click Validate URL to validate the path. Make sure that the validation is successful.
12. Click Apply to apply the changes.
13. Click Download Now to download the patch metadata immediately.![VUM](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/08/VUM-Conected.png?resize=790%2C429)
14. If you see an error “Incompatible repository. Provide a repository from vSphere Update Manager Download Service 5.x”, then VUM can connect to the UMDS server but you have misconfigured something. eg. No patches have been downloaded, wrong directory stated in URL, IIS not installed, etc)
15. Success. A single repository that can be used by all the VUMs in the environment.

To automated the download process you will need to create a Windows Scheduled Task. I used [@<span class="u-linkComplex-target">vDerekS</span>](https://twitter.com/vDerekS) blog post [Derek Seaman’s Blog](http://www.derekseaman.com/2011/11/schedule-vmware-umds-downloads-with.html) to complete this. The blog is a bit old but still valid for 2008(R2) and 2012 servers (With a bit of tweeking)