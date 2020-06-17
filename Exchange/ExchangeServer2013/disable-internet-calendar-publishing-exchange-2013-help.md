---
title: 'Disable Internet calendar publishing: Exchange 2013 Help'
TOCTitle: Disable Internet calendar publishing
ms:assetid: f26dbf04-9dae-460f-a987-2ad3dfbc7b7e
ms:mtpsurl: https://technet.microsoft.com/library/JJ853047(v=EXCHG.150)
ms:contentKeyID: 50390856
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Disable Internet calendar publishing

_**Applies to:** Exchange Server 2013_

How you disable Internet calendar publishing depends on how you enabled it. If you created a sharing policy dedicated to Internet calendar publishing, you can either disable the policy or delete it altogether. If you configured Internet calendar publishing as a sharing rule in the default sharing policy, you can just remove the sharing rule for the **Anonymous** domain.

When you disable Internet calendar publishing, users who are provisioned to use the sharing policy won't be able to share calendar information with the **Anonymous** internet domain specified in the policy. However, you can't delete or disable a sharing policy that's dedicated to Internet calendar publishing until all users who are provisioned to use that policy have the policy setting removed from their mailboxes. For details about changing the sharing policy setting for a user, see [Manage user mailboxes](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-user-mailboxes/manage-user-mailboxes).

> [!NOTE]
> If you disable or delete a sharing policy, users provisioned to use the policy will continue to share information until the Sharing Policy Assistant runs. To specify how often the Sharing Policy Assistant runs, use the <A href="https://docs.microsoft.com/powershell/module/exchange/Set-MailboxServer">Set-MailboxServer</A> cmdlet with the <EM>SharingPolicySchedule</EM> parameter.

To fully disable Internet calendar publishing, you should also disable the Outlook Web App virtual directory used for calendar publishing. Doing this prohibits access to the published calendar links previously shared by your Exchange organization users with external Internet users. This step is detailed later in this topic.

To learn more about Internet calendar publishing and sharing policies, see [Sharing](sharing-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Calendar and Sharing Permissions" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Use the EAC or the Shell to disable or delete the sharing policy for Internet calendar publishing

### Use the EAC

1. Navigate to **Organization** \> **Sharing**.

2. In the list view, under **Individual Sharing**, perform one of the following steps:

   - If you created a sharing policy specifically for Internet calendar publishing, select that policy, and then either clear the check box in the **On** column to disable the sharing policy or click **Delete** ![Delete icon](images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif "Delete icon") to delete it.

   - If you configured Internet calendar publishing as a sharing rule in the default sharing policy, perform the following steps:

     1. Select the default sharing policy, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

     2. In **sharing policy**, select the **Anonymous** sharing rule, and then click **Remove** ![Remove icon](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "Remove icon") to remove the sharing rule.

     3. Click **Save**.

### Use the Shell

This example disables a dedicated Internet calendar publishing sharing policy named **Internet**.

```powershell
Set-SharingPolicy -Identity "Internet" -Enabled $false
```

This example deletes a dedicated Internet calendar publishing sharing policy named **Internet**.

```powershell
Remove-SharingPolicy -Identity "Internet"
```

For detailed syntax and parameter information, see [Set-SharingPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-SharingPolicy).

### How do you know this step worked?

To verify that you have successfully removed or updated the sharing policy, run the following Shell command and verify the sharing policy information.

```powershell
Get-SharingPolicy <policy name> | format-list
```

If you've removed the dedicated Internet calendar publishing sharing policy, you won't see the policy in the cmdlet results.

If you've updated the default sharing policy, verify that the `Anonymous` domain has been removed from the *Domains* parameter.

For detailed syntax and parameter information, see [Get-SharingPolicy](https://docs.microsoft.com/powershell/module/exchange/Get-SharingPolicy).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 2: Use the Shell to disable the Outlook Web App virtual directory Anonymous features

> [!NOTE]
> You can't use the EAC to disable Anonymous features for the Outlook Web App virtual directory.

This example disables Anonymous features for the Outlook Web App virtual directory on Client Access server CAS01.

```powershell
Set-OwaVirtualDirectory -Identity "CAS01" - AnonymousFeaturesEnabled -$false
```

For detailed syntax and parameter information, see [Set-OwaVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Set-OwaVirtualDirectory).

### How do you know this step worked?

To verify that you have successfully disabled the Anonymous features for the Outlook Web App virtual directory on the Client Access server, run the following Shell command and verify that the *AnonymousFeaturesEnabled* parameter is `$false`.

```powershell
Get-OwaVirtualDirectory | format-list
```

For detailed syntax and parameter information, see [Get-OwaVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Get-OwaVirtualDirectory).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
