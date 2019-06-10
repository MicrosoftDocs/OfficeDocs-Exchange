---
title: 'Configure an accepted domain within your Exchange organization as authoritative'
TOCTitle: Configure an accepted domain within your Exchange organization as authoritative
ms:assetid: e182d54f-e58a-47ba-a5c1-28c0dfa86eed
ms:mtpsurl: https://technet.microsoft.com/en-us/library/JJ657734(v=EXCHG.150)
ms:contentKeyID: 49300733
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure an accepted domain within your Exchange organization as authoritative

_**Applies to:** Exchange Server 2013_

If a domain belonging to your organization hosts mailboxes for all the recipients within an SMTP namespace, that domain is considered to be authoritative. By default, one accepted domain is configured as authoritative for the Exchange organization. If your organization has more than one SMTP namespace, you can configure more than one accepted domain as authoritative.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Accepted domains" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- If you have a subscribed Edge Transport server in your perimeter network, you configure accepted domains on a Mailbox server in your Exchange organization. The accepted domains configuration is replicated to the Edge Transport server during EdgeSync synchronization. For more information, see [Edge Subscriptions](edge-subscriptions-exchange-2013-help.md).

- You can't create an accepted domain that has the same name as an already configured remote domain. For example, if you have fabrikam.com configured as a remote domain, you can't create an accepted domain for fabrikam.com.

- Before you configure an accepted domain, you must verify that a public Domain Name System (DNS) MX resource record for that SMTP namespace exists and that the MX resource record references a server name and an IP address associated with your Exchange organization.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to configure an accepted domain as authoritative

If an accepted domain for your Exchange organization hosts all the mailboxes for recipients within that domain's SMTP namespace, you may want to configure it as an authoritative domain.

1. In the EAC, navigate to **Mail flow** \> **Accepted domains**, and click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **Name** field, enter the display name for the accepted domain. Each accepted domain for your organization must have a unique display name This may be different than the accepted domain. For example, the domain contoso.com could have a display name of Contoso Local Accepted Domain.

3. In the **Accepted domain** field, enter the accepted domain. Specify an SMTP namespace for which your organization accepts email messages. (for example, Contoso.com).

4. Select **Authoritative domain**. This option is for email relayed to servers within your Exchange organization for an accepted domain that hosts mailboxes for all the recipients within an SMTP namespace.

5. Click **Save**.

> [!TIP]
> To configure an accepted domain that has already been created, select the domain from the accepted domains list and click <STRONG>Edit</STRONG> <IMG title="Edit icon" alt="Edit icon" src="images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif">. You can configure more than one domain as authoritative.

## How do you know this worked?

Your new accepted domain will appear in the accepted domains list in the EAC. To verify that you have successfully configured the accepted domain as authoritative, send mail to the domain and verify that it is received.
