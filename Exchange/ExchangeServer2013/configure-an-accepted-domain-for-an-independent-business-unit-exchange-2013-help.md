---
title: 'Configure an accepted domain for an independent business unit'
TOCTitle: Configure an accepted domain for an independent business unit
ms:assetid: bc95dbdc-3669-4c06-ab94-90093bc0dbfd
ms:mtpsurl: https://technet.microsoft.com/en-us/library/JJ657491(v=EXCHG.150)
ms:contentKeyID: 49289397
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure an accepted domain for an independent business unit

_**Applies to:** Exchange Server 2013_

In some situations you may want to configure an accepted domain for an independent business unit with email servers outside your Exchange organization. In such scenarios, you can configure the accepted domain as an external relay domain.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Accepted domains" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- If you have a subscribed Edge Transport server in your perimeter network, you configure accepted domains on a Mailbox server in your Exchange organization. The accepted domains configuration is replicated to the Edge Transport server during EdgeSync synchronization. For more information, see [Edge Subscriptions](edge-subscriptions-exchange-2013-help.md).

- You can't create an accepted domain that has the same name as an already configured remote domain. For example, if you have fabrikam.com configured as a remote domain, you can't create an accepted domain for fabrikam.com.

- Before you configure an accepted domain, you must verify that a public Domain Name System (DNS) MX resource record for that SMTP namespace exists and that the MX resource record references a server name and an IP address associated with your Exchange organization.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to configure the an accepted domain as an external relay domain

You may want to configure an accepted domain for a business unit with email servers outside your Exchange organization.

1. In the EAC, navigate to **Mail flow** \> **Accepted domains**, select the domain you wish to configure, and click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. In the **Name** field, enter the display name for the accepted domain. Each accepted domain for your organization must have a unique display name. This may be different than the accepted domain. For example, the domain Contoso.com could have a display name of Contoso Local Accepted Domain.

3. Select **External Relay Domain**. This option is for email is relayed to a server outside your Exchange organization.

4. Click **Save**.

## How do you know this worked?

To verify that you have successfully configured an accepted domain as an external relay domain, send a message from the accepted domain you've configured as an external relay domain, and verify that it is received.
