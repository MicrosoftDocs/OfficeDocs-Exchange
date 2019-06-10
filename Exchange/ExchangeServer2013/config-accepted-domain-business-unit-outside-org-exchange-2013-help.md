---
title: 'Configure accepted domain for business unit with mailboxes outside your Exchange organization'
TOCTitle: Configure an accepted domain for a business unit with mailboxes outside your Exchange organization
ms:assetid: ff46310b-5392-4eac-97bc-d39d397e1ce1
ms:mtpsurl: https://technet.microsoft.com/en-us/library/JJ657737(v=EXCHG.150)
ms:contentKeyID: 49300759
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure an accepted domain for a business unit with mailboxes outside your Exchange organization

_**Applies to:** Exchange Server 2013_

In some instances, you may want to configure an accepted domain for a business unit in which some or all recipients in the domain don't have mailboxes in your Exchange organization. This can occur, for example, when an organization shares the same SMTP address space between two or more different email systems. For such scenarios, you can configure an accepted domain as an internal relay domain.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Accepted domains" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- If you have a subscribed Edge Transport server in your perimeter network, you configure accepted domains on a Mailbox server in your Exchange organization. The accepted domains configuration is replicated to the Edge Transport server during EdgeSync synchronization. For more information, see [Edge Subscriptions](edge-subscriptions-exchange-2013-help.md).

- Before you configure an accepted domain, you must verify that a public Domain Name System (DNS) MX resource record for that SMTP namespace exists and that the MX resource record references a server name and an IP address associated with your Exchange organization.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to configure an accepted domain as an internal relay domain

1. In the EAC, navigate to **Mail flow** \> **Accepted domains**, select the domain you wish to configure, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. In the **Name** field, enter the display name for the accepted domain. Each accepted domain for your organization must have a unique display name. This may be different than the accepted domain. For example, the domain Contoso.com could have a display name of Contoso Local Accepted Domain.

3. Select **Internal Relay Domain**.

4. Click **Save**.

## How do you know this worked?

To verify that you have successfully configured an accepted domain as an internal relay domain, send a message from the internal relay domain to a mailbox within your Exchange organization and verify that it is received.
