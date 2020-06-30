---
title: 'Create a shared mailbox: Exchange 2013 Help'
TOCTitle: Create a shared mailbox
ms:assetid: d34bc827-1e83-4a7f-a219-8ba9c19fe24b
ms:mtpsurl: https://technet.microsoft.com/library/JJ150570(v=EXCHG.150)
ms:contentKeyID: 47560108
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a shared mailbox

_**Applies to:** Exchange Server 2013_

**Estimated time to complete: 5 minutes**

Shared mailboxes makes it easy for a group of people in your company to monitor and send email from a common account, such as info@contoso.com or support@contoso.com. When a person in the group replies to a message sent to the shared mailbox, the email looks like it was sent by the shared mailbox, not from the individual user.

> [!IMPORTANT]
> If you're using Microsoft 365 or Office 365, you should create your shared mailbox in the Microsoft 365 admin center.

If your organization uses a hybrid Exchange environment, you should use the on-premises Exchange admin center (EAC) to create and manage shared mailboxes. To learn more about shared mailboxes, see [Shared mailboxes](shared-mailboxes-exchange-2013-help.md).

## Use the EAC to create a shared mailbox

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "User mailboxes" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

1. Go to **Recipients** \> **Shared** \> **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. Fill-in the required fields:

   - **Display name**

   - **Email address**

3. To grant Full Access or Send As permissions, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"), and then select the users you want to grant permissions to. You can use the **CTRL** key to select multiple users. Confused about which permission to use? See Which permission should you use? later in this topic.

   > [!NOTE]
   > The Full Access permission allows a user to open the mailbox as well as create and modify items in it. The Send As permission allows anyone other than the mailbox owner to send email from this shared mailbox. Both permissions are required for successful shared mailbox operation.

4. Click **Save** to save your changes and create the shared mailbox.

## Use the EAC to edit shared mailbox delegation

1. Go to **Recipients** \> **Shared** \> **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. Click **Mailbox delegation**

3. To grant or remove Full Access and Send As permissions, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") or **Remove** ![Remove icon](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "Remove icon") and then select the users you want to grant permissions to.

   > [!NOTE]
   > The Full Access permission allows a user to open the mailbox as well as create and modify items in it. The Send As permission allows anyone other than the mailbox owner to send email from this shared mailbox. Both permissions are required for successful shared mailbox operation.

4. Click **Save** to save your changes.

## Use a shared mailbox

To learn how users can access and use shared mailboxes, check out the following:

- [Open and use a shared mailbox in Outlook](https://support.microsoft.com/office/d94a8e9e-21f1-4240-808b-de9c9c088afd)

- [Open and use a shared mailbox in Outlook Web App](https://support.microsoft.com/office/bc127866-42be-4de7-92ae-1ef2f787fd5c)

## Use the Shell to create a shared mailbox

This example creates the shared mailbox Sales Department and grants Full Access and Send on Behalf permissions for the security group MarketingSG. Users who are members of the security group will be granted the permissions to the mailbox.

> [!NOTE]
> This example assumes that you've already created the security group MarketingSG and that security group is mail-enabled. See <A href="https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-enabled-security-groups">Manage mail-enabled security groups</A>.

```powershell
New-Mailbox -Shared -Name "Sales Department" -DisplayName "Sales Department" -Alias Sales | Set-Mailbox -GrantSendOnBehalfTo MarketingSG | Add-MailboxPermission -User MarketingSG -AccessRights FullAccess -InheritanceType All
```

For detailed syntax and parameter information, see [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/New-Mailbox).

## Which permissions should you use?

You can use the following permissions with a shared mailbox.

- **Full Access**: The Full Access permission lets a user open the shared mailbox and act as the owner of that mailbox. After accessing the shared mailbox, a user can create calendar items; read, view, delete, and change email messages; create tasks and calendar contacts. However, a user with Full Access permission can't send email from the shared mailbox unless they also have Send As or Send on Behalf permission.

- **Send As**: The Send As permission lets a user impersonate the shared mailbox when sending mail. For example, if Kweku logs into the shared mailbox Marketing Department and sends an email, it will look like the Marketing Department sent the email.

- **Send on Behalf**: The Send on Behalf permission lets a user send email on behalf of the shared mailbox. For example, if John logs into the shared mailbox Reception Building 32 and sends an email, it look like the mail was sent by "John on behalf of Reception Building 32". You can't use the EAC to grant Send on Behalf permissions, you must use [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox) cmdlet with the *GrantSendonBehalf* parameter.

## More information

For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
