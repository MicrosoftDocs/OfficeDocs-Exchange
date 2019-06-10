---
title: 'Configure Exchange mail routing settings in Active Directory'
TOCTitle: Configure Exchange mail routing settings in Active Directory
ms:assetid: d01f8545-c201-4a96-be39-ed4c7008afcf
ms:mtpsurl: https://technet.microsoft.com/en-us/library/JJ674705(v=EXCHG.150)
ms:contentKeyID: 49351085
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure Exchange mail routing settings in Active Directory

_**Applies to:** Exchange Server 2013_

By default Microsoft Exchange Server 2013 references the IP site link objects in Active Directory to help determine the least-cost routing path. However, if you determine the Active Directory IP site link costs and traffic flow patterns aren't optimal for mail routing in Exchange, you can configure settings in Active Directory that are only used by Exchange to help optimize mail flow.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Active Directory site and site link management" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to configure an Exchange-specific cost on an Active Directory IP site link

Determine the name of the Active Directory IP site link for which you want to set an Exchange cost. A lower cost value indicates a more preferred route. You can examine the contents of the routing table logs and view the data in the **ADTopologyPath ID** section to view details about the calculated least cost routing path between two Active Directory sites.

To set an Exchange-specific cost on an Active Directory site link, run the following command:

```powershell
Set-AdSiteLink <ADSiteLinkIdentity> -ExchangeCost <Integer | $null>
```

This example sets an Exchange-specific cost of 10 on the IP site link named IPSiteLinkAB.

```powershell
Set-AdSiteLink IPSiteLinkAB -ExchangeCost 10
```

This example clears the Exchange cost from the IP site link named IPSiteLinkAB.

```powershell
Set-AdSiteLink IPSiteLinkAB -ExchangeCost $null
```

## How do you know this worked?

To verify that you have successfully set an Exchange cost on an Active Directory site link, do the following:

1. Run the following command:

   ```powershell
   Get-AdSiteLink | Format-List Name,ExchangeCost
   ```

2. Verify the Exchange cost is configured on the Active Directory site link.

## Use the Shell to configure an Active Directory site as a hub site

When a hub site exists along the least cost routing path for a message, the message must be routed through the hub site. Examine the contents of the routing table logs and view the data in the **ADTopologyPath ID** section to verify that the selected site exists along the least cost routing path between two Active Directory sites. If this isn't the case, you need to assign Exchange-specific costs to the IP site links to make the least cost routing path go through the selected sites.

To configure an Active Directory site as a hub site, run the following command:

```powershell
Set-AdSite <ADSiteIdentity> -HubSiteEnabled $true
```

This example configures the Active Directory site named Site A as a hub site.

```powershell
Set-AdSite "Site A" -HubSiteEnabled $true
```

This example removes the hub site attribute from the Active Directory site named Site B.

```powershell
Set-AdSite "Site B" -HubSiteEnabled $false
```

## How do you know this worked?

To verify that you have successfully configured an Active Directory site as a hub site, do the following:

1. Run the following command:

   ```powershell
   Get-AdSite | Format-List Name,HubSiteEnabled
   ```

2. Verify the *HubSiteEnabled* value is `True` for the Active Directory site.
