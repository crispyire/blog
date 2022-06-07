---
id: 157
title: 'vCloud Private cloud &#8211; Notes from the field'
date: '2015-06-15T09:00:56+01:00'
author: 'Christian Parker'
layout: post
guid: 'http://www.baylyparker.com/?p=157'
permalink: /2015/06/vcloud-private-cloud-notes-from-the-field/
amazon-product-content-location:
    - '1'
amazon-product-excerpt-hook-override:
    - '3'
amazon-product-content-hook-override:
    - '2'
amazon-product-newwindow:
    - '2'
amazon-product-isactive:
    - '1'
amazon-product-single-asin:
    - 978-0789751652
categories:
    - Cloud
    - VMware
tags:
    - Cloud
    - vCD
    - vcloud
    - VCP-Cloud
    - vmware
    - VUM
---

I was recently asked by my client to create a 100 host vCloud private cloud and I thought I would share with you my experiences.

Firstly a little of the setup:

- vCenter Server 5.5 (2442329)
- 100 HP ProLiant BL460c blades with ESXi 5.5 (2718055) installed by image file
- Update Manager 5.5
- vCloud Director 5.5.4
- vShield 5.5.4

As there is a very high churn rate expected for this vCloud Pod, we did not want to configure it to the supported maximums. This was to offer a level of buffer, so the hosts-per-cluster was set at a maximum of 28 hosts so 4 clusters were setup with HA and fully automated DRS enabled (DRS is a requirement of vCloud Director).

Once the cluster was created, by use of PowerCLI to add the hosts to the cluster. This was my first gotcha! PowerCLI is a fantastic method of interacting with vSphere but make sure you add the licencing lines with the script. As I found to my peril that the evaluation licence will get max’d out and your wonderfully carved script will stop working, and red error messages will start scrolling up your screen! I am not a great PowerCLI scripter, so I’m more than happy to say most of my script were 80% based on scripts put out by the VMware Community. Thanks Guys!

When the hosts had been added and properly licensed, I enabled the functionality of Host Profiles. Especially as you scale up your environments the likelihood of configuration mistakes becomes a more likely occurrence. Host Profiles gives the double benefits of ensuring all configurations are consistent throughout the environment but also a fast and effective method of deploying that constant configuration. Though all the physical hosts in this setup were identical and configuration was expected to stay identical in the clusters for additional future flexibility, a Host Profile was created per cluster.

Update Manager was also installed and all the hosts were brought up to a common patch level (In this case the latest patch level available from VMware). Update Manager (VUM), allows like Host Profiles to apply patches and updates in a far more consistent and larger scale.

When I was happy with the setup of the vSphere environment (Storage, compute and networking), I moved onto the vCloud Director (vCD). I have written another blog post that will be published soon on that process. That said, once installed, vCD requires an agent installed on the hosts and this can be configured from the vCD GUI. The first time I added the vSphere resources to vCD, I was scratching my head trying to work out why no hosts were populated in the fields. This is because vCD will not be able to draw on this information until you have configured the clusters to the setup. Once you have added in your first Provider VDC as part of that setup you can add the cluster and then the hosts in that cluster become visible to vCloud Director. When you are able to see the hosts you can then ‘prepare’ (Add the agent) to the hosts to become part of the vCloud.

[![Preparing hosts for vCD](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/vCloud_Prepare.png?resize=300%2C137)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/vCloud_Prepare.png)

The next item is to create a Provider vDC. The aim is to create a logicial segmentation of resources (Compute and Storage) based on the resource characteristic of a VMware vSphere Cluster or a Resource Pool.

[![PVCD_Add](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/PVCD_Add.png?resize=300%2C148)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/PVCD_Add.png)

Next, we will create a Organization that according to VMware is:

> *An Organization is the fundamental vCloud Director grouping that contains users, the vApps that they create, and the resources the vApps use. It is a top-level container in a cloud that contains one or more Organization Virtual Data Centers (Org vDCs) and Catalog entities. It owns all the virtual resources for a cloud instance and can have many Org vDCs.*

[![Org_add](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/Org_add.png?resize=300%2C187)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/Org_add.png)

After those groups have been made, an External Network needs to be created to allow virtual machines in the cloud to access the outside world (Internet).

[![External_Networking_add](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/External_Networking_add.png?resize=300%2C166)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/External_Networking_add.png)

NB. This is based on the port groups in vCenter.

Lastly, we need to create a Network Pool, which are *“a pool of network resources must be available. These network pools must be created in advance of the creation of Org and vApp networks. If they do not exist, the only network option available to an organization is the direct connect to the provider network.”*

[![Networking_add](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/Networking_add.png?resize=300%2C185)](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/06/Networking_add.png)

…so we are there. A private cloud has been created.

Though I have skimmed over a few of the topics, I hope this give an idea of the requirements when setting up a VMware private cloud.

It should be noted that though we have the foundations of the private cloud, I haven’t gone into the creation of the VMs/vApps themselves. In my client’s environment this done via a custom web interface based on API calls into vCloud Director/vSphere. As an alternative, I can also recommend the use of vRealize Automation to create self-service portals for your users to provision your VMs. I should say there in a native vCloud Director method of creation and administration of VMs/vApps but though suitable for system administrators is not very intuitive; hence the suggestion of vRealize Automation as a method to create user centric self-service portals.

\[AMAZONPRODUCTS asin=”978-0789751652″\]