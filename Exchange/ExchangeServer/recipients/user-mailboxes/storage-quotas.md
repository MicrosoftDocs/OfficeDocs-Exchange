---
localization_priority: Normal
description: 'Summary: How to set storage quotas on Exchange mailboxes.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5f5fe292-c80e-4a0b-b3e6-e193ea5171d0
ms.reviewer:
title: Configure storage quotas for a mailbox
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure storage quotas for a mailbox

You can use the Exchange admin center (EAC) or the Exchange Management Shell to customize the mailbox storage quotas for specific mailboxes. Storage quotas let you control the size of mailboxes and manage the growth of mailbox databases. When a mailbox reaches or exceeds a specified storage quota, Exchange sends a descriptive notification to the mailbox owner.

Storage quotas are typically configured on a per-database basis. This means that the quotas configured for a mailbox database apply to all mailboxes in that database. For more information about managing per-database mailbox settings, see [Manage mailbox databases in Exchange Server](../../architecture/mailbox-servers/manage-databases.md).

This topic shows you how to customize storage settings for a specific mailbox instead of using the storage settings from the mailbox database. For additional management tasks related to user mailboxes, see [Manage user mailboxes](user-mailboxes.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to set storage quotas for a mailbox

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to change the storage quotas for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page, click **Mailbox Usage**, and then click **More options**.

4. Click **Customize the settings for this mailbox**, and then set the following boxes. The value range for any of the storage quota settings is from 0 through 2047 gigabytes (GB).

   - **Issue a warning at (GB)**: This box displays the maximum storage limit before a warning is issued to the user. If the mailbox size reaches or exceeds the value specified, Exchange sends a warning message to the user.

     > [!IMPORTANT]
     > The message associated with the **Issue warning** quota won't be sent to the user unless the value of this setting is greater than 50% of the value specified in the **Prohibit send** quota. For example, if you set the **Prohibit send** quota to 8 MB, you must set the **Issue warning** quota to at least 4 MB. If you don't, the **Issue warning** quota message won't be sent.

   - **Prohibit send at (GB)**: If the mailbox size reaches or exceeds the specified limit, Exchange prevents the user from sending new messages and displays a descriptive error message.

   - **Prohibit send and receive at (GB)**: If the mailbox size reaches or exceeds the specified limit, Exchange prevents the mailbox user from sending new messages and won't deliver any new messages to the mailbox. Any messages sent to the mailbox are returned to the sender with a descriptive error message.

5. Click **Save** to save your changes.

## Use the Exchange Management Shell to configure storage quotas for a mailbox

This example sets the issue warning, prohibit send, and prohibit send and receive quotas for Joe Healy's mailbox to 24.5 GB, 24.75 GB, and 25 GB respectively.

> [!NOTE]
> To ensure that the custom settings for the mailbox are used rather than the mailbox database defaults, you must set the _UseDatabaseQuotaDefaults_ parameter to `$false`.

```PowerShell
Set-Mailbox -Identity "Joe Healy" -IssueWarningQuota 24.5gb -ProhibitSendQuota 24.75gb -ProhibitSendReceiveQuota 25gb -UseDatabaseQuotaDefaults $false
```

This example sets the issue warning, prohibit send, and prohibit send and receive quotas for Ayla Kol's mailbox to 900 megabytes (MB), 950 MB, and 1 GB respectively, and configures the mailbox to use custom settings.

```PowerShell
Set-Mailbox -Identity "Ayla Kol" -IssueWarningQuota 900mb -ProhibitSendQuota 950mb -ProhibitSendReceiveQuota 1gb -UseDatabaseQuotaDefaults $false
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

## How do you know this worked?

To verify that you've successfully set the storage quotas for a mailbox, do one of the following:

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to verify the storage quotas for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page, click **Mailbox Usage**, and then click **More options**.

4. Verify that **Customize the settings for this mailbox** is selected.

5. Verify the storage quota settings.

Or

In the Exchange Management Shell, replace \<Identity\> with the name, email address or alias of the mailbox, and run the following command:

```PowerShell
Get-Mailbox <Identity> | Format-List IssueWarningQuota,ProhibitSendQuota,ProhibitSendReceiveQuota,UseDatabaseQuotaDefaults
```
