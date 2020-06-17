---
title: 'Apply a sharing policy to mailboxes: Exchange 2013 Help'
TOCTitle: Apply a sharing policy to mailboxes
ms:assetid: dd4cc765-8469-4176-bb6e-d5b0f5235927
ms:mtpsurl: https://technet.microsoft.com/library/JJ657501(v=EXCHG.150)
ms:contentKeyID: 49289435
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Apply a sharing policy to mailboxes

_**Applies to:** Exchange Server 2013_

Sharing policies are a part of federated sharing and enable user-established, people-to-people sharing of calendar information with different types of external users. The sharing policy that an admin applies to the user's mailbox determines what level of access a user can share and with whom. If you don't change anything, then the default sharing policy applies to all users. If you create a new sharing policy, you have to apply that policy to mailboxes before it takes effect. A sharing policy can be applied to a single user mailbox or to multiple user mailboxes simultaneously. An admin can also disable a user's sharing policy to prevent external access to calendars.

To learn more about federated sharing, see [Sharing](sharing-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the *Recipient Provisioning Permissions* entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- A sharing policy must exist. For details, see [Create a sharing policy](create-a-sharing-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to apply a sharing policy to a single mailbox

1. Navigate to **recipients** \> **mailboxes**.

2. In the list view, select the mailbox you want, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In **User Mailbox**, click **mailbox features**.

4. In the **Sharing policy** list, select the sharing policy you want to apply to this mailbox.

5. Click **save** to apply the sharing policy.

## Use the EAC to apply a sharing policy to multiple mailboxes

1. Navigate to **recipients** \> **mailboxes**.

2. In the list view, hold the Ctrl key while you select multiple mailboxes..

3. In the details pane, the mailbox properties will be configured for bulk editing. Click **More options**.

4. Under **Sharing policy**, click **Update**.

5. In **bulk assign sharing policy**, use the **Select the sharing policy** list to select a sharing policy to assign to the mailboxes.

6. Click **save** to apply the sharing policy to the selected mailboxes.

## Use the Shell to apply a sharing policy to one or more mailboxes

This example applies the sharing policy Contoso to a single mailbox for the user Barbara.

```powershell
Set-Mailbox -Identity Barbara -SharingPolicy "Contoso"
```

This example specifies that all user mailboxes in the Marketing department use the sharing policy Contoso Marketing.

```powershell
Get-Mailbox -Filter "Department -eq 'Marketing'" | Set-Mailbox -SharingPolicy "Contoso Marketing"
```

This example returns all mailboxes that have the sharing policy Contoso applied, and it sorts the users into a table that displays only their aliases and email addresses.

```powershell
Get-Mailbox -ResultSize unlimited | Where {$_.SharingPolicy -eq "Contoso" } | format-table Alias, EmailAddresses
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox) and [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox).

## How do you know this worked?

To verify that you have successfully applied the sharing policy to a user mailbox, do one of the following:

- In the EAC, navigate to **Recipients** \> **Mailboxes**, and then select the mailbox to which you applied the sharing policy. Click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon"), click **mailbox features**, and then confirm that the correct sharing policy appears in the **Sharing policy** list.

- Run the following Shell command to verify the sharing policy was assigned to a user mailbox. Verify that the correct sharing policy is listed in the *SharingPolicy* parameter.

  ```powershell
  Get-Mailbox <user name> | format-list
  ```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
