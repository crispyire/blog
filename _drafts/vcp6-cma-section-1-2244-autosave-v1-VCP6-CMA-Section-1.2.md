---
id: 290
title: 'VCP6-CMA | Section 1.2'
date: '2015-09-15T15:35:18+01:00'
author: 'Christian Parker'
layout: revision
guid: 'http://www.baylyparker.com/2015/09/244-autosave-v1/'
permalink: '/?p=290'
---

## **VCP6-CMA – Objective 1.2:** Install and Upgrade vRealize Suite Components

## Deploy and configure appliances for distributed vRealize deployment (e.g. SSO, automation, DB)

A vRealize Automation installation includes installing and configuring single sign-on (SSO) capabilities, the user interface portal, and Infrastructure as a Service (IaaS) components.

#### **VMware Identity Appliance**

Identity Appliance is a pre-configured virtual appliance that provides single sign-on (SSO) capabilities for the vRealize Automation environment![Deploy OVF](https://i0.wp.com/www.baylyparker.com/wp-content/uploads/2015/09/DeployOVF.png?resize=150%2C150)

#### **VMware vRealize Appliance**

The vRealize Appliance is a pre-configured virtual appliance that deploys the vRealize Automation server. This forms the back-end of the user interface and IaaS portal.

#### **Appliance Database**

During deployment of the virtual appliances, the PostgreSQL database is created automatically on the first vCloud Automation Center Appliance. A system administrator can install the database on a separate server or on multiple servers to create a high-availability environment.

### Install IaaS components

- The administrator installs a complete set of infrastructure (IaaS) components on a Windows machine (physical or virtual). Administrator rights are required to perform these tasks. A minimal installation installs all of the components on the same Windows server, except for the SQL database, which you can install on a separate server.
- The Windows Server much be running either:

Microsoft Windows Server 2008 R2  
<span style="line-height: 1.5;">Microsoft Windows Server 2012</span>

<span style="line-height: 1.5;">and the supported database instances are:</span>

Microsoft SQL Server 2008 x64 (Only vRA 6.2.2)  
Microsoft SQL Server 2012 and 2014 – x64 (vRA 6.2, 2.2.1, 6.2.2)

### Configure default tenant and any additional tenants

Each tenant requires at least one identity store. Identity stores can be OpenLDAP or Active Directory. Active Directory in native mode is supported for the default tenant only. The default tenant is automatically created when you configure single sign-on. It will use the default SSO login @vsphere.local (Unless you have changed the domain in your SSO setup)

The default tenant is then created with the built-in system administrator account to log in to the vRealize Automation console. The system administrator can then configure the default tenant and create additional tenants

### Appoint administrators

You can appoint one or more tenant administrators and IaaS administrators from the identity stores you configured for a tenant. Tenant administrators are responsible for configuring tenant-specific branding, as well as managing identity stores, users, groups, entitlements, and shared blueprints within the context of their tenant. IaaS Administrators are responsible for configuring infrastructure source endpoints in IaaS, appointing fabric administrators, and monitoring IaaS logs.

1. ```
    Select Administration > Tenants.
    ```
2. ```
    Click the name of the default tenant, vsphere.local.
    ```
3. ```
    Click the Administrators tab.
    ```
4. ```
    Enter the name of a user or group in the Tenant Administrators search box and press Enter. For faster results, enter the entire user or group name, for example myAdmins@mycompany.domain. Repeat this step to appoint additional tenant administrators.
    ```
5. ```
    If you have installed IaaS, enter the name of a user or group in the Infrastructure Administrators search box and press Enter. For faster results, enter the entire user or group name, for example IaaSAdmins@mycompany.domain. Repeat this step to appoint additional infrastructure administrators.
    ```
6. ```
    Verify that the user or group names you chose appear in Tenant Administrators and Infrastructure Administrators lists.
    ```
7. ```
    Click Update.
    ```

### Configure load balancer

After you deploy the appliances for vRealize Automation, you can set up a load balancer to distribute traffic among multiple instances of the vRealize Appliance. The following list provides an overview of the general steps required to configure a load balancer for vRealize Automation traffic:

1. ```
    Install your load balancer.
    ```
2. ```
    Enable session affinity, also known as sticky sessions.
    ```
3. ```
    Ensure that the timeout on the load balancer is at least 100 seconds.
    ```
4. ```
    If your network or load balancer requires it, import a certificate to your load balancer.
    ```
5. ```
    Configure the load balancer for vRealize Appliance traffic.
    ```
6. ```
    Configure the load balancer to forward port 5480.
    ```
7. ```
    Configure the appliances for vRealize Automation
    ```

### Integrate vRealize with external systems

vRealize Automation uses agents to integrate with external systems. A system administrator can select agents to install to communicate with other virtualization platforms. vRealize Automation uses the following types of agents to manage external systems:

- Hypervisor proxy agents (vSphere, Citrix Xen Servers and Microsoft Hyper-V servers)
- External provisioning infrastructure (EPI) integration agents
- Virtual Desktop Infrastructure (VDI) agents
- Windows Management Instrumentation (WMI) agents

### Manage SSL certificates

vRealize Automation uses SSL certificates for secure communication among IaaS components, the Identity Appliance, and instances of the vRealize Appliance. The appliances and the Windows installation machines exchange these certificates to establish a trusted connection. You can obtain certificates from an internal or external certificate authority, or generate self-signed certificates during the deployment process for each component.

The specific implementation of the certificates required to achieve this trust depends on your environment. To provide high availability and failover support, you might deploy load balanced clusters of components. In this case, you obtain a multi-use certificate that includes each component in the cluster, and then copy that multi-use certificate to each component in the cluster. You can use Subject Alternative Name (SAN) certificates, chain certificates, wildcard certificates, or any other method of multi-use certification appropriate for your environment as long as you satisfy the trust requirements. Depending on your load balancer configuration, you may need to certify the load balancer as part of the multi-use certificate for the cluster.

For example, if you have a load balancer configuration that requires a certificate on the load balancer as well as its components, you might obtain a SAN certificate to certify web-load-balancer.eng.mycompany.com, web-component-1.eng.mycompany.com, and web-component-2.eng.mycompany.com. You would copy that single multi-use certificate to the load balancer and each of the appliances and then register the certificate on the Web component machines.

### Resolve deployment and configuration issues

Time to get the lab out 🙂

### Perform upgrade of vCAC 6.1 to vRealize Automation

You upgrade incrementally from vRealize Automation 6.x until you reach the latest vRealize Automation.

Locate your currently installed version in the table and then follow the steps in the documents on the right to incrementally upgrade your vRealize Automation environment to the latest release. You can find links to the documentation for all versions of vCloud Automation Center and vRealize Automation at: <https://www.vmware.com/support/pubs/vcac-pubs.html> and [Upgrading to vRealize from vCAC 6.1](http://pubs.vmware.com/vra-62/topic/com.vmware.ICbase/PDF/vrealize-automation-62-upgrading.pdf)

### Download and install updates to vRealize component appliances

A good description can be found here on the process of updating: <http://kb.vmware.com/kb/2091012>

Upgrade IaaS components

See <http://pubs.vmware.com/vCAC-61/index.jsp?topic=%2Fcom.vmware.vcac.61upgrade.doc%2FGUID-0FF1B1BE-2162-4404-8EA9-BA5ED415B060.html>

## VMware Documentation

- [vRealize Automation Installation and Configuration Guide](http://pubs.vmware.com/vra-62/topic/com.vmware.ICbase/PDF/vrealize-automation-62-installation-and-configuration.pdf)
- [Upgrading to vRealize from vCAC 6.1](http://pubs.vmware.com/vra-62/topic/com.vmware.ICbase/PDF/vrealize-automation-62-upgrading.pdf)