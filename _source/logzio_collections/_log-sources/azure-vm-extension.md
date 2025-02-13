---
title: Ship logs with Azure VM extension
logo:
  logofile: azure-vm.png
  orientation: vertical
data-source: Azure VM extension
templates: ["azure-deployment"]

contributors:
  - nshishkin
shipping-tags:
  -  azure
order: 460
---

Extensions are small applications that provide post-deployment configuration and automation on Azure VMs. You can install Logz.io agents on Azure virtual machines as an extension. This will allow you to ship logs directly from your VM to your Logz.io account.

<!-- info-box-start:info -->
Logz.io Azure VM extension currently only supports Linux-based VMs.
{:.info-box.note}
<!-- info-box-end -->


<div class="tasklist">
  
**Before you begin, you'll need**: 

* Logz.io app installed from your Azure Marketplace.
* An active account with Logz.io.
* Resource group created under your Logz.io account in Azure.
  
  
##### Login to Logz.io from your Azure account
  
Log in to Logz.io using either the SSO in your Azure account or a link to the Logz.io platform.
  
If SSO is enabled for Logz.io in Azure:
  
![SSO enabled](https://dytvr9ot2sszz.cloudfront.net/logz-docs/azure_extension/sso-opt-in-extension.png)

If SSO is not enabled for Logz.io in Azure:
  
![SSO not enabled](https://dytvr9ot2sszz.cloudfront.net/logz-docs/azure_extension/sso-opt-out-extension.png) 

##### Navigate to the Virtual machine agent

Select **Logz configuration > Virtual machine agent**.
  
![VM agent](https://dytvr9ot2sszz.cloudfront.net/logz-docs/azure_extension/vm-agent-extension.png)


##### Install the extension for the required VM
  
Select the VM that you need to install the extension on, and click **Install Agent**. Confirm that the extension will be installed with a default configuration.
  
##### Verify the installation

To verify that the Logz.io agent was installed, select the VM and navigate to the **Extensions** window.
  
##### Run the VM

Run the VM to generate logs.
  
##### Check Logz.io for your data

Give your data some time to get from your system to ours, and then open [Kibana](https://app.logz.io/#/dashboard/kibana).

If you still don't see your logs, see [log shipping troubleshooting]({{site.baseurl}}/user-guide/log-shipping/log-shipping-troubleshooting.html).
  
##### Uninstall the extension

To uninstall the Logz.io extension from a VM , select the VM and click **Uninstall agent**.

</div>



