---
id: 276
title: 'Configuring Netgear READYNAS  for VMware ESXi 5.x'
date: '2015-09-13T22:33:26+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2015/09/20-revision-v1/'
permalink: '/?p=276'
---

While preparing for my VMware VCAP-DCA exam, I have being using a number of virtual NFS and iSCSI devices but a few weeks ago I bought a Netgear ReadyNAS to form part of my lab environment.

Though I have found a number of articles/blogs regarding setup, what I found is that Netgear’s firmware updates have made all the screen not mesh while trying to complete the setup. This is my attempt at showing how to do the setup and hopefully avoiding the mistakes and frustrations along the way.

So to make things clear, I am using the Netgear ReadyNAS 104 with firmware 6.2.2

![ReadyNAS_Status](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_Status.png?resize=300%2C114)

So, firstly we need to create the NFS share on the ReadyNAS.

This is done via the ReadyNAS webclient. Once you have logged in, click \[Shares\] and you should see a screen similar to the one below. Once you do, click the \[New Folder\] button and follow the prompts:

![ReadyNAS_Admin](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_Admin.png?resize=300%2C78)

Give your new folder that is going to store for new VMware datastore a meaningful name and some description so you know what it is for. Select the \[NFS\] option, as this is what we will be used for connecting our ESXi hosts to it. If you want to access in parallel via other means select the other options you wish but the \[NFS\] option is mandatory for this exercise.

[![ReadyNAS_NewFolder](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_NewFolder.png?resize=300%2C194)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_NewFolder.png)

When you are ready, select \[Create\]

[![ReadyNAS_NewFolderSuccess](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_NewFolderSuccess.png?resize=300%2C155)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_NewFolderSuccess.png)

Success! Select \[OK\] to complete the folder setup.

We will now give your VMware hosts access to the new NFS folder by selecting the folder from the list, and then selecting \[Settings\]

[![RreadyNAS_FolderSettings](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/RreadyNAS_FolderSettings.png?resize=300%2C129)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/RreadyNAS_FolderSettings.png)

Select the plus icon to add the VMware hosts you want to give access to the folder

[![RreadyNAS_HostAdd](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/RreadyNAS_HostAdd.png?resize=300%2C98)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/RreadyNAS_HostAdd.png)

Add in the IP address of the ALL the VMware hosts to access this NFS folder. And then select \[Add\]

[![ReadyNAS_AddHost](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_AddHost.png?resize=300%2C135)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_AddHost.png)

The IP address should now be listed in the list.

NB. You must select \[READ/WRITE\] and \[ROOT ACCESS\] or when adding the NFS folder in VMware you will encounter and error and not be able to mount the datastore. Select \[Ok\] when you are done this.

[![ReadyNAS_HostRoot](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_HostRoot.png?resize=300%2C163)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ReadyNAS_HostRoot.png)

That’s the ReadyNAS NFS folders all setup! Step 1 complete!

Let us now switch to our VMware host.

On the host, select \[Configuration\], \[Storage\] and then \[Add Storage\]

[![ESXiHost](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ESXiHost.png?resize=300%2C52)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ESXiHost.png)

Select \[Network File System\] from the options, and \[Next\]

[![NFS](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/NFS.png?resize=300%2C234)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/NFS.png)

Type the IP address of your ReadyNAS in the \[Server\] window

The folder should also include the volume name and not just the share. Go back to your ReadyNAS webclient to confirm this. In my case the volume name is “Data” and my folder is called “ESXiDatastore”. Back in the VMware NFS wizard, the format should then be in my case “/data/ESXiDatstore”

[![RreadyNAS_volumes](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/RreadyNAS_volumes.png?resize=300%2C298)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/RreadyNAS_volumes.png)

Lastly, give the datastore a name. This is what will show in vCenter, so make it meaningful to yourself. Since I only have one NFS share, my lack of imagination called it “NFS”. Once this is complete select \[Next\]

[![NFSAdd](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/NFSAdd.png?resize=300%2C236)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/NFSAdd.png)

Congratulations, you now have your NFS Datastore attached to the host and can be used for virtual machine storage, ISO repository any other VMware storage uses you may have.

[![ESXIHost_NFS](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ESXIHost_NFS.png?resize=300%2C137)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/01/ESXIHost_NFS.png)

The process will need to be repeated for all hosts in your environment.

I hope that this is been helpful to you… And as this is my first real blog, I hope it makes sense!