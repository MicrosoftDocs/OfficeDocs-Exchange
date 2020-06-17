---
title: 'Create a discovery mailbox: Exchange 2013 Help'
TOCTitle: Create a discovery mailbox
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: bc20285d-35e2-4e49-9bd3-38abf96114ba
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a discovery mailbox in Exchange 2013

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup creates a discovery mailbox by default. Discovery mailboxes are used as target mailboxes for [In-Place eDiscovery](in-place-ediscovery-exchange-2013-help.md) searches in the Exchange admin center (EAC). You can create additional discovery mailboxes as required. After you create a new discovery mailbox, you will have to assign Full Access permissions to the appropriate users so they can access eDiscovery search results that are copied to the discovery mailbox.

> [!CAUTION]
> After a discovery manager copies the results of an eDiscovery search to a discovery mailbox, the mailbox can potentially contain sensitive information. You should control access to discovery mailboxes and make sure only authorized users can access them.

For more information, see [Discovery mailboxes](in-place-ediscovery-exchange-2013-help.md#discovery-mailboxes).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Creating discovery mailboxes" entry in [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- Discovery mailboxes have a mailbox storage quota of 50 gigabytes (GB). This storage quota can't be increased.

- You can't use the EAC to create a discovery mailbox or assign permissions to access it. You have to use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Step 1: Create a discovery mailbox

This example creates a discovery mailbox named SearchResults in the Shell.

```powershell
New-Mailbox -Name SearchResults -Discovery
```

For detailed syntax and parameter information, see [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/new-mailbox).

To display a list of all discovery mailboxes in an Exchange organization, run the following command:

```powershell
Get-Mailbox -Resultsize unlimited -Filter "RecipientTypeDetails -eq 'DiscoveryMailbox'"
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox).

## Step 2: Assign permissions to a discovery mailbox

You have to explicitly assign users or groups the necessary permissions to open a discovery mailbox that you've created. Use the following syntax to assign a user or group permissions to open a discovery mailbox and view search results:

```powershell
Add-MailboxPermission <Name of the discovery mailbox> -User <Name of user or group> -AccessRights FullAccess -InheritanceType all
```

For example, the following command assigns the Full Access permission to the Litigation Managers group, so members of the group can open the Fabrikam Litigation discovery mailbox.

```powershell
Add-MailboxPermission "Fabrikam Litigation" -User "Litigation Managers" -AccessRights FullAccess -InheritanceType all
```

For detailed syntax and parameter information, see [Add-MailboxPermission](https://docs.microsoft.com/powershell/module/exchange/add-mailboxpermission).

## More information

- By default, members of the Discovery Management role group only have Full Access permission to the default Discovery Search Mailbox. You will have to explicitly assign the Full Access permission to the Discovery Management role group if you want members to open a discovery mailbox that you've created.

- Although visible in Exchange address lists, users can't send email to a discovery mailbox. Email delivery to discovery mailboxes is prohibited with delivery restrictions. This preserves the integrity of search results copied to a discovery mailbox.

- A discovery mailbox can't be repurposed or converted to another type of mailbox.

- You can remove a discovery mailbox as you would any other type of mailbox.
