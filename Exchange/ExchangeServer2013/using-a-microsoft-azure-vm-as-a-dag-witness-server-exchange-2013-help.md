---
title: 'Using a Microsoft Azure VM as a DAG witness server: Exchange 2013 Help'
TOCTitle: Using a Microsoft Azure VM as a DAG witness server
ms:assetid: 03d1e215-518b-4b48-bfcd-8d187ff8f5ef
ms:mtpsurl: https://technet.microsoft.com/library/Dn903504(v=EXCHG.150)
ms:contentKeyID: 63886642
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Using a Microsoft Azure VM as a DAG witness server

_**Applies to:** Exchange Server 2013_

Exchange Server 2013 enables you to configure your mailbox databases in a database availability group (DAG) for automatic datacenter failover. This configuration requires three separate physical locations: two datacenters for mailbox servers and a third location to place the witness server for the DAG. Organizations with only two physical locations now can also take advantage of automatic datacenter failover by using a Microsoft Azure file server virtual machine to act as the DAG's witness server.

This article focuses on the placement of the DAG witness on Microsoft Azure and assumes that you are familiar with site resilience concepts and already have a fully functional DAG infrastructure spanning two datacenters. If you don't already have your DAG infrastructure configured, we recommend that you first review the following articles:

[High availability and site resilience](high-availability-and-site-resilience-exchange-2013-help.md)

[Database availability groups (DAGs)](database-availability-groups-dags-exchange-2013-help.md)

[Planning for high availability and site resilience](planning-for-high-availability-and-site-resilience-exchange-2013-help.md)

## Changes to Microsoft Azure

This configuration requires a multi-site VPN. It has always been possible to connect your organization's network to Microsoft Azure using a site-to-site VPN connection. However, in the past, Azure supported only a single site-to-site VPN. Since configuring a DAG and its witness across three datacenters required multiple site-to-site VPNs, placement of the DAG witness on an Azure VM wasn't initially possible.

In June 2014, Microsoft Azure introduced multi-site VPN support, which enabled organizations to connect multiple datacenters to the same Azure virtual network. This change also made it possible for organizations with two datacenters to leverage Microsoft Azure as a third location to place their DAG witness servers. To learn more about the multi-site VPN feature in Azure, see [Configure a Multi-Site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site).

> [!NOTE]
> This configuration leverages Azure virtual machines and a multi-site VPN for deploying the witness server and does not use the Azure Cloud Witness.

## Microsoft Azure file server witness

The following diagram is an overview of using a Microsoft Azure file server VM as a DAG witness. You need an Azure virtual network, a multi-site VPN that connects your datacenters to your Azure virtual network, and a domain controller and a file server deployed on Azure virtual machines.

> [!NOTE]
> It is technically possible to use a single Azure VM for this purpose and place the file witness share on the domain controller. However, this will result in an unnecessary elevation of privileges. Therefore, it is not a recommended configuration.

### DAG witness server on Microsoft Azure

![Exchange DAG witness on Azure overview](images/Dn903504.7cbda882-bbae-4be7-b0ea-60947b8aa4ef(EXCHG.150).png "Exchange DAG witness on Azure overview")

The first thing you need to do in order to use a Microsoft Azure VM for your DAG witness is to get a subscription. See [How to buy Azure](https://azure.microsoft.com/pricing/purchase-options/) for the best way to acquire an Azure subscription.

After you have your Azure subscription, you need to do the following in order:

1. Prepare the Microsoft Azure virtual network

2. Configure a multi-site VPN

3. Configure virtual machines

4. Configure the DAG witness

> [!NOTE]
> A significant portion of the guidance in this article involves Microsoft Azure configuration. Therefore, links to Azure documentation is used whenever applicable.

## Prerequisites

  - Two datacenters that are capable of supporting an Exchange high availability and site resilience deployment. See [Planning for high availability and site resilience](planning-for-high-availability-and-site-resilience-exchange-2013-help.md) for more information

  - A public IP address that is not behind NAT for the VPN gateways in each site

  - A VPN device in each site that is compatible with Microsoft Azure. See [About VPN devices and IPsec/IKE parameters for Site-to-Site VPN Gateway connections](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) for more information about compatible devices

  - Familiarity with DAG concepts and management

  - Familiarity with Windows PowerShell

## Phase 1: Prepare the Microsoft Azure virtual network

Configuring the Microsoft Azure network is the most crucial part of the deployment process. At the end of this phase, you will have a fully functional Azure virtual network that is connected to your two datacenters via a multi-site VPN.

## Register DNS servers

Because this configuration requires name resolution between the on-premises servers and Azure VMs, you will need to configure Azure to use your own DNS servers. [Name resolution for resources in Azure virtual networks](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances) topic provides an overview of name resolution in Azure.

Do the following to register your DNS servers:

1. In the Azure portal, go to **networks**, and then click **NEW**.

2. Click **NETWORK SERVICES** \> **VIRTUAL NETWORK** \> **REGISTER DNS SERVER**.

3. Type the name and IP address for your DNS server. The name specified here is a logical name used in the management portal and doesn't have to match the actual name of your DNS server.

4. Repeat steps 1 through 3 for any other DNS servers you want to add.

    > [!NOTE]
    > The DNS servers you register are not used in a round robin fashion. Azure VMs will use the first DNS server listed and will only use any additional servers if the first one is not available.

5. Repeat steps 1 through 3 to add the IP address you will use for the domain controller you will deploy on Microsoft Azure.

## Create local (on-premises) network objects in Azure

Next, do the following to create logical network objects that represent your datacenters in Microsoft Azure:

1. In the Azure portal, and then go to **networks**, and then click **NEW**.

2. Click **NETWORK SERVICES** \> **VIRTUAL NETWORK** \> **ADD LOCAL NETWORK**.

3. Type the name for your first datacenter site and the IP address of the VPN device on that site. This IP address must be a static public IP address that is not behind NAT.

4. On the next screen, specify the IP subnets for your first site.

5. Repeat steps 1through 4 for your second site.

## Create the Azure virtual network

Now, do the following to create an Azure virtual network that will be used by the VMs:

1. In the Azure portal, go to **networks**, and then click **NEW**.

2. Click **NETWORK SERVICES** \> **VIRTUAL NETWORK** \> **CUSTOM CREATE**.

3. On the **Virtual Network Details** page, specify a name for the virtual network, and select a geographic location for the network.

4. In the **DNS Servers and VPN Connectivity** page, verify that the DNS servers you previously registered are listed as the DNS servers.

5. Select the **Configure a site-to-site VPN** check box under **SITE-TO-SITE CONNECTIVITY**.

    > [!IMPORTANT]
    > Do not select <STRONG>Use ExpressRoute</STRONG> because this will prevent the necessary configuration changes required to set up a multi-site VPN.

6. Under **LOCAL NETWORK**, select one of the two on-premises networks you configured.

7. In the **Virtual Network Address Spaces** page, specify the IP address range you will use for your Azure virtual network.

## Checkpoint: Review the network configuration

At this point, when you go to **networks**, you should see the virtual network you configured under **VIRTUAL NETWORKS,** your local sites under **LOCAL NETWORKS**, and your registered DNS servers under **DNS SERVERS**.

## Phase 2: Configure a multi-site VPN

The next step is to establish the VPN gateways to your on-premises sites. To do this, you need to:

1. Establish a VPN gateway to one of your sites by using the Azure portal.

2. Export the virtual network configuration settings.

3. Modify the configuration file for multi-site VPN.

4. Import the updated Azure network configuration.

5. Record the Azure gateway IP address and preshared keys.

6. Configure on-premises VPN devices.

For more information about configuring a multi-site VPN, see [Configure a Multi-Site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site).

## Establish a VPN gateway to your first site

When creating your virtual gateway, note that you already specified that it will be connected to your first on-premises site. When you go into the virtual network dashboard, you will see that the gateway has not been created.

To establish the VPN gateway on the Azure side, see [VPN Gateway](https://azure.microsoft.com/services/vpn-gateway/) 

> [!IMPORTANT]
> Only perform the steps in the "Start the virtual network gateway" section of the article, and do not continue to the subsequent sections.

## Export virtual network configuration settings

The Azure management portal doesn't currently allow you to configure a multi-site VPN. For this configuration, you need to export the virtual network configuration settings to an XML file and then modify that file. Follow the instructions at[
Create a virtual network (classic) by using the Azure porta](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-vnet-classic-pportal) to export your settings.

## Modify the network configuration settings for the multi-site VPN

Open the file you exported in any XML editor. The gateway connections to your on-premises sites are listed in the "ConnectionsToLocalNetwork" section. Search for that term in the XML file to locate the section. This section in the configuration file will look like the following (assuming the site name you created for your local site is "Site A").

```powershell
<ConnectionsToLocalNetwork>

    <LocalNetworkSiteRef name="Site A">

        <Connection type="IPsec" />

</LocalNetworkSiteRef>
```

To configure your second site, add another "LocalNetworkSiteRef" section under the "ConnectionsToLocalNetwork" section. The section in the updated configuration file will look like the following (assuming the site name for your second local site is "Site B").

```powershell
<ConnectionsToLocalNetwork>

        <LocalNetworkSiteRef name="Site A">

            <Connection type="IPsec" />

        <LocalNetworkSiteRef name="Site B">

            <Connection type="IPsec" />

    </LocalNetworkSiteRef>
```

Save the updated configuration settings file.

## Import virtual network configuration settings

The second site reference you've added to the configuration file will trigger Microsoft Azure to create a new tunnel. Import the updated file using the instructions in [Create a virtual network (classic) by using the Azure portal](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-vnet-classic-pportal). After you complete the import, the virtual network dashboard will show the gateway connections to both of your local sites.

## Record the Azure gateway IP address and pre-shared keys

After the new network configuration settings are imported, the virtual network dashboard will display the IP address for the Azure gateway. This is the IP address that the VPN devices on both of your sites will connect to. Record this IP address for reference.

You also will need to get the pre-shared IPsec/IKE keys for each tunnel that was created. You will use these keys along with the Azure gateway IP address to configure your on-premises VPN devices.

You need to use PowerShell to get the pre-shared keys. If you aren't familiar with using PowerShell to manage Azure, see [Azure PowerShell](https://docs.microsoft.com/powershell/azure/?view=azps-3.0.0).

Use the [Get-AzureVNetGatewayKey](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azurevnetgatewayke) cmdlet to extract the pre-shared keys. Run this cmdlet once for each tunnel. The following example shows the commands you need to run to extract the keys for tunnels between the virtual network "Azure Site" and sites "Site A" and "Site B." In this example, the outputs are saved into separate files. Alternatively, you can pipeline these keys to other PowerShell cmdlets or use them in a script.

```powershell
Get-AzureVNETGatewayKey -VNetName "Azure Site" -LocalNetworkSiteName "Site A" > C:\Keys\KeysForTunnelToSiteA.txt

Get-AzureVNETGatewayKey -VNetName "Azure Site" -LocalNetworkSiteName "Site B" > C:\Keys\KeysForTunnelToSiteB.txt
```

## Configure on-premises VPN devices

Microsoft Azure provides VPN device configuration scripts for supported VPN devices. Click the **Download VPN Device Script** link on the virtual network dashboard for the appropriate script for your VPN devices.

The script you download will have the configuration setting for the first site that you configured when you set up your virtual network, and can be used as is to configure the VPN device for that site. For example, if you specified Site A as the **LOCAL NETWORK** when you created your virtual network, the VPN device script can be used for Site A. However, you will need to modify it to configure the VPN device for Site B. Specifically, you need to update the pre-shared key to match the key for the second site.

For example, if you are using a Routing and Remote Access Service (RRAS) VPN device for your sites, you will need to:

1. Open the configuration script in any text editor.

2. Find the `#Add S2S VPN interface` section.

3. Find the **Add-VpnS2SInterface** command in this section. Verify that the value for the *SharedSecret* parameter matches the pre-shared key for the site for which you are configuring the VPN device.

Other devices might require additional verifications. For example, the configuration scripts for Cisco devices set ACL rules by using the local IP address ranges. You need to review and verify all references to the local site in the configuration script before you use it. 

## Checkpoint: Review the VPN status

At this point, both of your sites are connected to your Azure virtual network through the VPN gateways. You can validate the status of the multi-site VPN by running the following command in PowerShell.

```powershell
Get-AzureVnetConnection -VNetName "Azure Site" | Format-Table LocalNetworkSiteName, ConnectivityState
```

If both tunnels are up and running, the output of this command will look like the following.

```powershell
LocalNetworkSiteName    ConnectivityState

--------------------    -----------------

Site A                  Connected

Site B                  Connected
```

You can also verify connectivity by viewing the virtual network dashboard in the Azure management portal. The **STATUS** column for both sites will show as **Connected**.

> [!NOTE]
> It can take several minutes after the connection is successfully established for the status change to appear in the Azure management portal.

## Phase 3: Configure virtual machines

You need to create a minimum of two virtual machines in Microsoft Azure for this deployment: a domain controller and a file server that will serve as the DAG witness.

1. Create virtual machines for your domain controller and your file server using the instructions in [Quickstart: Create a Windows virtual machine in the Azure portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal). Make sure that you select the virtual network you created for **REGION/AFFINITY GROUP/VIRTUAL NETWORK** when specifying the settings of your virtual machines.

2. Specify preferred IP addresses for both the domain controller and the file server using Azure PowerShell. When you specify a preferred IP address for a VM, it needs to be updated, which will require restarting the VM. The following example sets the IP addresses for Azure-DC and Azure-FSW to 10.0.0.10 and 10.0.0.11 respectively.

    ```powershell
    Get-AzureVM Azure-DC | Set-AzureStaticVNetIP -IPAddress 10.0.0.10 | Update-AzureVM

    Get-AzureVM Azure-FSW | Set-AzureStaticVNetIP -IPAddress 10.0.0.11 | Update-AzureVM
    ```

    > [!NOTE]
    > A VM with a preferred IP address will attempt to use that address. However, if that address has been assigned to a different VM, the VM with the preferred IP address configuration will not start. To avoid this situation, make sure that the IP address you use isn't assigned to another VM. See <A href="https://docs.microsoft.com/azure/virtual-network/virtual-networks-static-private-ip-arm-pportal">Configure private IP addresses for a virtual machine using the Azure portal</A> for more information.

3. Provision the domain controller VM on Azure using the standards used by your organization.

4. Prepare the file server with the prerequisites for an Exchange DAG witness:

    1. Add the File Server role using the Add Roles and Features Wizard or the [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature) cmdlet.

    2. Add the Exchange Trusted Subsystems universal security group to the Local Administrators group.

## Checkpoint: Review virtual machine status

At this point, your virtual machines should be up and running and should be able to communicate with servers in both of your on-premises datacenters:

  - Verify that your domain controller in Azure is replicating with your on-premises domain controllers.

  - Verify that you can reach the file server on Azure by name and establish an SMB connection from your Exchange servers.

  - Verify that you can reach your Exchange servers by name from the file server on Azure.

## Phase 4: Configure the DAG witness

Finally, you need to configure your DAG to use the new witness server. By default, Exchange uses the C:\\DAGFileShareWitnesses as the file share witness path on your witness server. If you are using a custom file path, you should also update the witness directory for the specific share.

1. Connect to Exchange Management Shell.

2. Run the following command to configure the witness server for your DAGs.

    ```powershell
    Set-DatabaseAvailabilityGroup -Identity DAG1 -WitnessServer Azure-FSW
    ```

See the following topics for more information:

[Configure database availability group properties](configure-database-availability-group-properties-exchange-2013-help.md)

[Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/set-databaseavailabilitygroup)

## Checkpoint: Validate the DAG file share witness

At this point, you have configured your DAG to use the file server on Azure as your DAG witness. Do the following to validate your configuration:

1. Validate the DAG configuration by running the following command.

    ```powershell
    Get-DatabaseAvailabilityGroup -Identity DAG1 -Status | Format-List Name, WitnessServer, WitnessDirectory, WitnessShareInUse
    ```

    Verify that the *WitnessServer* parameter is set to the file server on Azure, the *WitnessDirectory* parameter is set to the correct path, and the *WitnessShareInUse* parameter shows **Primary**.

2. If the DAG has an even number of nodes, the file share witness will be configured. Validate the file share witness setting in cluster properties by running the following command. The value for the *SharePath* parameter should point to the file server and display the correct path.

    ```powershell
    Get-ClusterResource -Cluster MBX1 | Get-ClusterParameter | Format-List
    ```

3. Next, verify the status of the "File Share Witness" cluster resource by running the following command. The *State* of the cluster resource should display **Online**.

    ```powershell
    Get-ClusterResource -Cluster MBX1
    ```

4. Lastly, verify that the share is successfully created on the file server by reviewing the folder in File Explorer and the shares in Server Manager.

## See Also

[Planning for high availability and site resilience](planning-for-high-availability-and-site-resilience-exchange-2013-help.md)
[Switchovers and Failovers](switchovers-and-failovers-exchange-2013-help.md)
[Managing database availability groups](managing-database-availability-groups-exchange-2013-help.md)
