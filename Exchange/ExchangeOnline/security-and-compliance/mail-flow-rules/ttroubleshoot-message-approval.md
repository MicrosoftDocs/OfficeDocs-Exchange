---
localization_priority: Normal
description: Learn how to delete an arbitration mailbox that's being used by mailboxes in Exchange Online
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 860df43f-a05b-4da3-83f1-68d3123a923d
ms.reviewer: 
f1.keywords:
- NOCSH
title: Manage and troubleshoot message approval in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars
ROBOTS: NOINDEX, NOFOLLOW

---

# Manage and troubleshoot message approval in Exchange Online

You may receive the following error when you attempt to remove an arbitration mailbox:

 `Can't remove the arbitration mailbox <mailbox> because it's being used for the approval workflow for existing recipients that have either membership restrictions or moderation enabled. You should either disable the approval features on those recipients or specify a different arbitration mailbox for those recipients before removing this arbitration mailbox.`

An arbitration mailbox can be used to handle the approval workflow for moderated recipients and distribution group membership approvals. You use PowerShell to find all the recipients that are configured to use the arbitration mailbox. After you identify the recipients, you can either configure them to use a different arbitration mailbox, or you can disable moderation for them.

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes

- You need to be assigned permissions before you can perform these procedures. To see what permissions you need, see the "Aribtration" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

## Step 1: Use Exchange Online PowerShell to find all the recipients that use the arbitration mailbox you are trying to delete

Run the following commands:

```PowerShell
$AM = Get-Mailbox "<arbitration mailbox>" -Arbitration
$AMDN = $AM.DistinguishedName
Get-Recipient -RecipientPreviewFilter "ArbitrationMailbox -eq '$AMDN'"
```

For example, to find all the recipients that use the arbitration mailbox named Arbitration Mailbox01, run the following commands:

```PowerShell
$AM = Get-Mailbox "Arbitration Mailbox01" -Arbitration
$AMDN = $AM.DistinguishedName
Get-Recipient -RecipientPreviewFilter "ArbitrationMailbox -eq '$AMDN'"
```

> [!NOTE]
> The arbitration mailbox is specified using the distinguished name (DN). If you know the DN of the arbitration mailbox, you can run the single command: `Get-Recipient -RecipientPreviewFilter "ArbitrationMailbox -eq '<DN>'"`.

## Step 2: Use Exchange Online PowerShell to specify a different arbitration mailbox or disable moderation for the recipients

To stop moderated recipients from using the arbitration mailbox you are trying to delete, you can either specify a different arbitration mailbox, or you can disable moderation for the recipients.

If you choose to specify a different arbitration mailbox for the recipients, run the following command:

```PowerShell
Set-<RecipientType> <Identity> -ArbitrationMailbox <different arbitration mailbox>
```

For example, to reconfigure the distribution group named All Employees to use the arbitration mailbox named Arbitration Mailbox02 for membership approval, run the following command:

```PowerShell
Set-DistributionGroup "All Employees" -ArbitrationMailbox "Arbitration Mailbox02"
```

If you choose to disable moderation for the recipients, run the following command:

```PowerShell
Set-<RecipientType> <Identity> -ModerationEanbled $false
```

For example, to disable moderation for the mailbox named Human Resources, run the following command:

```PowerShell
Set-Mailbox "Human Resources" -ModerationEanbled $false
```

## How do you know this worked?

The procedure was successful if you can delete the arbitration mailbox without receiving the error that it's being used.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
