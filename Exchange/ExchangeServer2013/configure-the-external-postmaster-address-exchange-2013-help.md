---
title: 'Configure the external postmaster address: Exchange 2013 Help'
TOCTitle: Configure the external postmaster address
ms:assetid: 6b0c8675-3238-462d-8973-b52305fb90d2
ms:mtpsurl: https://technet.microsoft.com/library/Bb430765(v=EXCHG.150)
ms:contentKeyID: 49318579
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the external postmaster address

_**Applies to:** Exchange Server 2013_

The external postmaster address is used as the sender for system-generated messages and notifications sent to message senders that exist outside of your Microsoft Exchange Server 2013 organization. An external sender is any sender that has an email address in a domain that isn't configured as an accepted domain in your organization.

By default, the value of the external postmaster address setting is blank. This default value causes the following behavior in your Exchange organization:

- The external postmaster address is postmaster@\<*Default accepted domain*\> for all Mailbox servers and subscribed Edge Transport servers.

- The external postmaster address is postmaster@\<*Edge Transport server FQDN*\> for all unsubscribed Edge Transport servers.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport configuration" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- When you configure a custom external postmaster address, that value applies to all Exchange 2013 Mailbox servers and Exchange 2010 Hub Transport servers in your Exchange organization. However, that value isn't replicated to Edge Transport servers. If you specify a custom value for the external postmaster address, you need to manually configure the external postmaster address value on any Edge Transport servers.

- If you have any Exchange 2007 Hub Transport servers or Edge Transport servers in your organization, you need to configure the custom external postmaster address on each one of those servers using the **Set-TransportServer** cmdlet. For more information, see [Managing the External Postmaster Address](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb430765(v=exchg.80)).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to configure the external postmaster address

1. In the EAC, navigate to **Mail flow** \> **Receive connectors** \> **More options** ![More Options Icon](images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif "More Options Icon") \> **Organization transport settings** \> **Delivery** tab.

2. In the **External postmaster address** field, enter the SMTP email address, for example, `postmaster@contoso.com`. If you want to return the external postmaster address to the default value, delete any existing value so the field is blank.

3. When you're finished, click **Save**.

## Use the Shell to configure the external postmaster address

To configure the external postmaster address, use the following syntax.

```powershell
Set-TransportConfig -ExternalPostmasterAddress <postmaster address>
```

For example, to set the external postmaster address to the value `postmaster@contoso.com`, run the following command

```powershell
Set-TransportConfig -ExternalPostmasterAddress postmaster@contoso.com
```

To return the external postmaster address to the default value, run the following command:

```powershell
Set-TransportConfig -ExternalPostmasterAddress $null
```

## How do you know this worked?

To verify that you have successfully configured the external postmaster address, do the following:

1. Run the following command on a Mailbox server to verify the external postmaster address value:

    ```powershell
    Get-TransportConfig | Format-List ExternalPostmasterAddress
    ```

2. From an external email account, send a message to your Exchange organization that will generate a delivery status notification (DSN). For example, you can configure a transport rule to send a non-delivery report (NDR) for a message from that sender that contains specific keywords. Verify the sender's email address in the DSN matches the value you specified.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
