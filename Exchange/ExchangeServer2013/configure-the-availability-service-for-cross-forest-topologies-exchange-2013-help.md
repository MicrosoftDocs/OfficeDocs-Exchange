---
title: 'Configure the Availability service for cross-forest topologies'
TOCTitle: Configure the Availability service for cross-forest topologies
ms:assetid: f1e7d407-f0d3-47a7-8cc3-03c5980445d5
ms:mtpsurl: https://technet.microsoft.com/library/Bb125182(v=EXCHG.150)
ms:contentKeyID: 51588094
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the Availability service for cross-forest topologies

_**Applies to:** Exchange Server 2013_

The Availability service improves information workers' free/busy information by providing secure, consistent, and up-to-date free/busy information to clients that are running Microsoft Outlook. By default, this service is installed with Exchange Server 2013. In cross-forest topologies where all connecting clients are running Outlook, the Availability service is the only method of retrieving free/busy information. You can use the Shell to configure the Availability service for cross-forest topologies.

> [!NOTE]
> You can't use the EAC to configure the Availability service for cross-forest topologies.

## Using the Availability service in trusted and untrusted forests

You can use the Availability service in cross-forest topologies across trusted or untrusted forests. The type of free/busy information that's available depends on if you're using a trusted or untrusted forest.

**Trusted forests**: In trusted forests, you can configure the Availability service to retrieve free/busy information on a per-user basis. When the Availability service is configured to retrieve free/busy information on a per-user basis, the service can make cross-forest requests on behalf of a particular user. This allows a user in a remote forest to retrieve detailed free/busy information for someone who is not in the same forest.

**Untrusted forests**: In untrusted forests, you can only configure the Availability service to retrieve free/busy information on an organization-wide basis. When the Availability service makes free/busy cross-forest requests at the organizational level, free/busy information is returned for each user in the organization. In untrusted forests, it isn't possible to control the level of free/busy information that's returned on a per-user basis.

## Configuring Windows for cross-forest topologies

By default, a global address list (GAL) contains mail recipients from a single forest. If you have a cross-forest environment, we recommend using Microsoft Identity Lifecycle Manager (ILM) 2007 Feature Pack 1 (FP1) to ensure that the GAL in any given forest contains mail recipients from other forests. ILM 2007 FP1 creates mail users that represent recipients from other forests, thereby allowing users to view them in the GAL and send mail. For example, users in Forest A appear as a mail user in Forest B and vice versa. Users in the target forest can then select the mail user object that represents a recipient in another forest to send mail.

To enable GAL synchronization, you create management agents that import mail-enabled users, contacts, and groups from designated Active Directory services into a centralized metadirectory. In the metadirectory, mail-enabled objects are represented as mail users. Groups are represented as contacts without any associated membership. The management agents then export these mail users to an organizational unit in the specified target forest.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Availability Service Permissions" entries in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to configure per-user free/busy information in a trusted cross-forest topology

This example configures the Availability service to retrieve per-user free/busy information on a Mailbox server in the target forest.

```powershell
Get-MailboxServer | Add-ADPermission -Accessrights Extendedright -Extendedrights "ms-Exch-EPI-Token-Serialization" -User "<Remote Forest Domain>\Exchange servers"
```

This example defines the free/busy access method that the Availability service uses on the local Mailbox server in the source forest. The local Mailbox server is configured to access free/busy information from the forest ContosoForest.com on a per-user basis. This example uses the service account to retrieve free/busy information.

```powershell
Add-AvailabilityAddressSpace -Forestname ContosoForest.com -AccessMethod PerUserFB -UseServiceAccount:$true
```

> [!NOTE]
> To configure bidirectional cross-forest availability, repeat these steps in the target forest.

If you choose to configure cross-forest availability with trust, and also choose to use a service account (instead of specifying organization-wide or per-user credentials), you must extend permissions as shown in the example in the "Use the Shell to configure trusted cross-forest availability with a service account" section. Performing that procedure in the target forest gives Mailbox servers in the source forest permission to serialize the original user context.

## Use the Shell to configure trusted cross-forest availability with a service account

This example configures trusted cross-forest availability with a service account.

Get-MailboxServer | Add-ADPermission -Accessrights Extendedright -Extendedright "ms-Exch-EPI-Token-Serialization" -User "<Remote Forest Domain>\Exchange servers"

For detailed information about syntax and parameters, see the following topics:

- [Get-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxServer)

- [Add-ADPermission](https://docs.microsoft.com/powershell/module/exchange/Add-ADPermission)

- [Add-AvailabilityAddressSpace](https://docs.microsoft.com/powershell/module/exchange/Add-AvailabilityAddressSpace)

- [Set-AvailabilityConfig](https://docs.microsoft.com/powershell/module/exchange/Set-AvailabilityConfig)

## Use the Shell to configure organization-wide free/busy information in an untrusted cross-forest topology

This example sets the organization-wide account on the availability configuration object to configure the access level for free/busy information in the target forest.

```powershell
Set-AvailabilityConfig -OrgWideAccount "Contoso.com\User"
```

This example adds the Availability address space configuration object for the source forest.

```powershell
$a = Get-Credential (Enter the credentials for organization-wide user in Contoso.com domain)
Add-AvailabilityAddressspace -Forestname Contoso.com -Accessmethod OrgWideFB -Credential:$a
```
