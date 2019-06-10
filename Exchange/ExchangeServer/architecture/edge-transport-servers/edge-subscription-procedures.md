---
localization_priority: Normal
description: After you've subscribed an Edge Transport server to an Active Directory site in your Exchange organization as described in Edge Subscriptions, you might need to perform maintenance tasks on the Edge Subscription. These tasks are described in this topic.
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: 27de4104-fb8e-4eab-9ad2-a64f81a4fb69
ms.date: 7/3/2018
ms.reviewer: 
title: Procedures for Edge Subscriptions
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Procedures for Edge Subscriptions

After you've subscribed an Edge Transport server to an Active Directory site in your Exchange organization as described in [Edge Subscriptions](edge-subscriptions.md), you might need to perform maintenance tasks on the Edge Subscription. These tasks are described in this topic.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "EdgeSync" entry and the "Edge Transport servers" section in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Remove an Edge Subscription

You may occasionally want to remove an Edge Subscription from the Exchange organization or from both the Exchange organization and the Edge Transport server. If you plan to later resubscribe the Edge Transport server to the Exchange organization, don't remove the Edge Subscription from the Edge Transport server. When you remove the Edge Subscription from an Edge Transport server, all replicated data is deleted from AD LDS. This can take a long time if you have lots of recipient data.

To completely remove an Edge Subscription, you need to run this procedure on the Edge Transport server you wish to remove and on an Exchange 2016 or Exchange 2019 Mailbox server in the Active Directory site where the Edge Transport server is subscribed.

After you remove the Edge Subscription, synchronization of information from AD LDS stops. All accounts stored in AD LDS are removed, and the Edge Transport server is removed from the source server list of any Send connector. You will no longer be able to use Edge Transport server features that rely on Active Directory data.

1. To remove the Edge Subscription from the Edge Transport server, use the following syntax.

   ```
   Remove-EdgeSubscription <EdgeTransportServerIdentity>
   ```

   For example, to remove the Edge Subscription on the Edge Transport server named Edge01, run the following command.

   ```
   Remove-EdgeSubscription Edge01
   ```

2. To remove the Edge Subscription from the Mailbox server, use the following syntax.

   ```
   Remove-EdgeSubscription <EdgeTransportServerIdentity>
   ```

   For example, to remove the Edge Subscription for the Edge Transport server named Edge01 on a Mailbox server in the subscribed Active Directory site, run the following command.

   ```
   Remove-EdgeSubscription Edge01
   ```

You will need to remove the Edge Subscription if:

- You no longer want the Edge Transport server to participate in EdgeSync synchronization. You will need to remove the Edge Subscription from both the Edge Transport server and from the Exchange organization.

- An Edge Transport server is being decommissioned. In this scenario, you only need to remove the Edge Subscription from the Exchange organization. If you uninstall the Edge Transport server role from the computer, the AD LDS instance and all Active Directory data stored in AD LDS will also be removed.

- You want to change the Active Directory site association for the Edge Subscription. You will only need to remove the Edge Subscription from the Exchange organization. After the Edge Subscription is removed from the Exchange organization, you can resubscribe the Edge Transport server to a different Active Directory site.

When you remove an Edge Subscription from the Exchange organization:

- Synchronization of information from Active Directory to AD LDS stops.

- The ESRA accounts are removed from both Active Directory and AD LDS.

- The Edge Transport server is removed from the _SourceTransportServers_ property of any Send connector.

- The automatic inbound Send connector from the Edge Transport server to the Exchange organization is removed from AD LDS.

When you remove the Edge Subscription from an Edge Transport server:

- You can no longer use Edge Transport server features that rely on Active Directory data.

- Replicated data is removed from AD LDS.

- Tasks that were disabled when the Edge Subscription was created are re-enabled to allow for local configuration.

## Resubscribe an Edge Transport server

Occasionally you may have to resubscribe an Edge Transport server to an Active Directory site. When the Edge Subscription is re-created, new credentials are generated and you need to follow the complete Edge Subscription process. You will need to resubscribe an Edge Transport server if:

- You add new Mailbox servers in the subscribed Active Directory site, and you want the new Mailbox server to participate in EdgeSync synchronization.

- You applied the license key for the Edge Transport server after creating the Edge Subscription. Licensing information for the Edge Transport server is captured when the Edge Subscription is created. Subscribed Edge Transport servers only appear as licensed if they are subscribed to the Exchange organization after the license key has already been applied on the Edge Transport server. If the license key is applied on the Edge Transport server after you perform the Edge Subscription process, the licensing information won't be updated in the Exchange organization, and you will need to resubscribe the Edge Transport server.

- The ESRA credentials are compromised.

> [!IMPORTANT]
> To resubscribe an Edge Transport server, export a new Edge Subscription file on the Edge Transport server and then import the XML file on a Mailbox server. You will need to resubscribe the Edge Transport server to the same Active Directory site where it was originally subscribed. You don't need to first remove the original Edge Subscription; the resubscription process will overwrite the existing Edge Subscription.

## Add or Remove a Mailbox server

If you add a Mailbox server to an Active Directory site that already has an Edge Transport server subscribed, the new Mailbox server doesn't automatically participate in EdgeSync synchronization. To enable a newly deployed Mailbox server to participate in EdgeSync synchronization, you need to resubscribe each Edge Transport server to the Active Directory site.

Removing a Mailbox server from an Active Directory site where an Edge Transport server is subscribed won't affect EdgeSync synchronization unless that Mailbox server is the only Mailbox server in that site. If you remove all Mailbox servers from the Active Directory site where an Edge Transport server is subscribed, that site's subscribed Edge Transport servers are orphaned.

## Run EdgeSync manually

You may want to manually run EdgeSync if you've made significant changes to the configuration or recipients in Active Directory and want your changes synchronized immediately. You can run a full synchronization, or only synchronize changes made since the last replication.

A manual EdgeSync resets the EdgeSync synchronization schedule. The next automatic synchronization is based on when you ran the manual synchronization.

To manually run EdgeSync, use the following syntax.

```
Start-EdgeSynchronization [-Server <MailboxServerIdentity>] [-TargetServer <EdgeTransportServerIdentity> [-ForceFullSync]
```

The following example starts EdgeSync with the following options:

- The synchronization is initiated from the Exchange Mailbox server named Mailbox01.

- All Edge Transport servers are synchronized.

- Only the changes since the last replication are synchronized.

```
Start-EdgeSynchronization -Server Mailbox01
```

This example starts EdgeSync with the following options:

- The synchronization is initiated from the local Mailbox server.

- Only the Edge Transport server named Edge03 is synchronized.

- All recipient and configuration data are fully synchronized.

```
Start-EdgeSynchronization -TargetServer Edge03 -ForceFullSync
```

## Verify EdgeSync results

You can use the **Test-EdgeSynchronization** cmdlet to verify that the Edge synchronization is working. This cmdlet reports synchronization status of subscribed Edge Transport servers.

The output of this cmdlet lets you view objects that have not been synchronized to the Edge Transport server. The task compares data stored in Active Directory against data stored in AD LDS and reports any data inconsistencies.

You can use the _ExcludeRecipientTest_ parameter on the **Test-EdgeSynchronization** cmdlet to exclude validation of recipient data synchronization. If you include this parameter, only the synchronization of configuration objects is validated. Validating recipient data will take longer than validating only configuration data.

## Verify EdgeSync results for a single recipient

To verify EdgeSync results for a single recipient, use the following syntax on a Mailbox server in the subscribed Active Directory site.

```
Test-EdgeSynchronization -VerifyRecipient <emailaddress>
```

This example verifies EdgeSync results for the user kate@contoso.com.

```
Test-EdgeSynchronization -VerifyRecipient kate@contoso.com
```
