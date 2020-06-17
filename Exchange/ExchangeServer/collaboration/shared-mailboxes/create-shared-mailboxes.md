---
localization_priority: Normal
description: If your organization uses a hybrid Exchange environment, you should use the on-premises Exchange admin center (EAC) to create and manage shared mailboxes.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d34bc827-1e83-4a7f-a219-8ba9c19fe24b
ms.reviewer:
title: Create shared mailboxes in the Exchange admin center
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create shared mailboxes in the Exchange admin center

If your organization uses a hybrid Exchange environment, you should use the on-premises Exchange admin center (EAC) to create and manage shared mailboxes.

## Use the EAC to create a shared mailbox

For information on limitations, automapping, and getting your users set up, see [Create a shared mailbox](https://docs.microsoft.com/microsoft-365/admin/email/create-a-shared-mailbox).

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "User mailboxes" entry in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

1. Go to **Recipients** \> **Shared** \> **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

2. Fill-in the required fields:

   - **Display name**

   - **Email address**

3. To grant Full Access or Send As permissions, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png), and then select the users you want to grant permissions to. You can use the CTRL key to select multiple users. Confused about which permission to use? See [Which permissions should you use?](#which-permissions-should-you-use) later in this topic.

   > [!NOTE]
   > The Full Access permission allows a user to open the mailbox as well as create and modify items in it. The Send As permission allows anyone other than the mailbox owner to send email from this shared mailbox. Both permissions are required for successful shared mailbox operation.

4. Click **Save** to save your changes and create the shared mailbox.

### Use the EAC to edit shared mailbox delegation

1. Go to **Recipients** \> **Shared** \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

2. Click **Mailbox delegation**

3. To grant or remove Full Access and Send As permissions, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png) or **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png) and then select the users you want to grant permissions to.

   > [!NOTE]
   > The Full Access permission allows a user to open the mailbox as well as create and modify items in it. The Send As permission allows anyone other than the mailbox owner to send email from this shared mailbox. Both permissions are required for successful shared mailbox operation.

4. Click **Save** to save your changes.

## Use the Exchange Management Shell to create a shared mailbox

This example creates the shared mailbox Sales Department and grants Full Access and Send on Behalf permissions for the security group MarketingSG. Users who are members of the security group will be granted the permissions to the mailbox.

> [!NOTE]
> This example assumes that you've already created the security group MarketingSG and that security group is mail-enabled. See [Manage mail-enabled security groups in Exchange Server](../../recipients/mail-enabled-security-groups.md).

```PowerShell
New-Mailbox -Shared -Name "Sales Department" -DisplayName "Sales Department" -Alias Sales | Set-Mailbox -GrantSendOnBehalfTo MarketingSG | Add-MailboxPermission -User MarketingSG -AccessRights FullAccess -InheritanceType All
```

For detailed syntax and parameter information, see [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/new-mailbox).

## Which permissions should you use?

You can use the following permissions with a shared mailbox.

- **Full Access**: The Full Access permission lets a user log into the shared mailbox and act as the owner of that mailbox. While logged in, the user can create calendar items; read, view, delete, and change email messages; create tasks and calendar contacts. However, a user with Full Access permission can't send email from the shared mailbox unless they also have Send As or Send on Behalf permission.

- **Send As**: The Send As permission lets a user impersonate the shared mailbox when sending mail. For example, if Kweku logs into the shared mailbox Marketing Department and sends an email, it will look like the Marketing Department sent the email.

- **Send on Behalf**: The Send on Behalf permission lets a user send email on behalf of the shared mailbox. For example, if John logs into the shared mailbox Reception Building 32 and sends an email, it look like the mail was sent by "John on behalf of Reception Building 32". You can't use the EAC to grant Send on Behalf permissions, you must use **Set-Mailbox** cmdlet with the _GrantSendonBehalf_ parameter.

## More information

For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
