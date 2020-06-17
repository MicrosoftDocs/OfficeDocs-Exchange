---
localization_priority: Normal
description: Learn how to use retention policies to manage an email lifecycle in Exchange Server. Retention policies are applied by creating retention tags, adding them to a retention policy, and applying the policy to mailbox users.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d8806c98-fea5-492f-906d-f514e25361b2
ms.reviewer:
title: Create a retention policy in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create a retention policy in Exchange Server

Learn how to use retention policies to manage an email lifecycle in Exchange 2016 and Exchange 2019. Retention policies are applied by creating retention tags, adding them to a retention policy, and applying the policy to mailbox users.

## What do you need to know before you begin?

- Estimated time to complete this task: 30 minutes.

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- Mailboxes to which you apply retention policies must reside on servers running Exchange Server 2010 or later.

## Step 1: Create a retention tag

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

 **Use the Exchange admin center (EAC) to create a retention tag**

1. Go to **Compliance management** \> **Retention tags**, and click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

2. Select one of the following options:

   - **Applied automatically to entire mailbox (default)**: Creates a default policy tag (DPT). You can use DPTs to create a default deletion policy and a default archive policy, which applies to all items in the mailbox.

     > [!NOTE]
     > You can't use the EAC to create a DPT to delete voice mail items. For details about how to create a DPT to delete voice mail items, see the Exchange Management Shell example below.

   - **Applied automatically to a default folder**: Creates a retention policy tag (RPT) for a default folder such as **Inbox** or **Deleted Items**.

     > [!NOTE]
     > You can only create RPTs with the **Delete and allow recovery** or **Permanently delete** actions.

   - **Applied by users to items and folders (personal)**: Creates personal tags. These tags allow Outlook and Outlook on the web users to apply archive or deletion settings to a message or folders that are different from the settings applied to the parent folder or the entire mailbox.

3. The **New retention tag** page title and options vary depending on the type of tag you select. Complete the following fields:

   - **Name**: Enter a name for the retention tag. Retention tag names are displayed to users in Outlook and Outlook on the web along with the retention period.

   - **Apply this tag to the following default folder**: Available only if you selected this option in Step 2.

   - **Retention action**: Select one of the following actions to take after the item reaches its retention period:

   - **Delete and Allow Recovery**: Deletes items but allow users to recover them using the **Recover Deleted Items** option in Outlook or Outlook on the web. Items are retained until the deleted item retention period configured for the mailbox database or the mailbox user is reached.

   - **Permanently Delete**: Permanently deletes the item from the mailbox database.

     > [!IMPORTANT]
     > Mailboxes or items subject to In-Place Hold or litigation hold will be retained and returned in In-Place eDiscovery searches. To learn more, see [In-Place Hold and Litigation Hold in Exchange Server](../../policy-and-compliance/holds/holds.md).

   - **Move to Archive**: Available only if you're creating a DPT or a personal tag. Select this action to move items to the user's In-Place Archive.

   - **Retention period**: Select one of the following options:

   - **Never**: Specifies that items should never be deleted or moved to the archive.

   - **When the item reaches the following age (in days)**: Specifies the number of days to retain items before they're moved or deleted. The retention age for all supported items except Calendar and Tasks is calculated from the date an item is received or created. Retention age for Calendar and Tasks items is calculated from the end date.

   - **Comment**: Optional field used for administrative notes or comments. The field isn't displayed to users.

 **Use the Exchange Management Shell to create a retention tag**

Use the **New-RetentionPolicyTag** cmdlet to create a retention tag. Different options available in the cmdlet allow you to create different types of retention tags. Use the _Type_ parameter to create a DPT (`All`), RPT (specify a default folder type, such as `Inbox`) or a personal tag (`Personal`).

This example creates a DPT to delete all messages in the mailbox after 7 years (2,556 days).

```PowerShell
New-RetentionPolicyTag -Name "DPT-Corp-Delete" -Type All -AgeLimitForRetention 2556 -RetentionAction DeleteAndAllowRecovery
```

This example creates a DPT to move all messages to the In-Place Archive in 2 years (730 days).

```PowerShell
New-RetentionPolicyTag -Name "DPT-Corp-Move" -Type All -AgeLimitForRetention 730 -RetentionAction MoveToArchive
```

This example creates a DPT to delete voice mail messages after 20 days.

```PowerShell
New-RetentionPolicyTag -Name "DPT-Corp-Voicemail" -Type All -MessageClass Voicemail -AgeLimitForRetention 20 -RetentionAction DeleteAndAllowRecovery
```

This example creates a RPT to permanently delete messages in the Junk EMail folder after 30 days.

```PowerShell
New-RetentionPolicyTag -Name "RPT-Corp-JunkMail" -Type JunkEmail -AgeLimitForRetention 30 -RetentionAction PermanentlyDelete
```

This example creates a personal tag to never delete a message.

```PowerShell
New-RetentionPolicyTag -Name "Never Delete" -Type Personal -RetentionAction DeleteAndAllowRecovery -RetentionEnabled $false
```

## Step 2: Create a retention policy

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

 **Use the EAC to create a retention policy**

1. Go to **Compliance management** \> **Retention policies**, and click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

2. In **New Retention Policy**, complete the following fields:

   - **Name**: Enter a name for the retention policy.

   - **Retention tags**: Click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png) to select the tags you want to add to this retention policy.

     A retention policy can contain the following tags:

     - One DPT with the **Move to Archive** action

     - One DPT with the **Delete and Allow Recovery** or **Permanently Delete** actions

     - One DPT for voice mail messages with the **Delete and Allow Recovery** or **Permanently Delete** actions

     - One RPT per default folder such as **Inbox** to delete items

     - Any number of personal tags

      > [!NOTE]
      > Although you can add any number of personal tags to a retention policy, having many personal tags with different retention settings can confuse users. We recommend linking no more than ten personal tags to a retention policy.

   You can create a retention policy without adding any retention tags to it, but items in the mailbox to which the policy is applied won't be moved or deleted. You can also add and remove retention tags from a retention policy after you create it.

### Use the Exchange Management Shell to create a retention policy

This example creates the retention policy RetentionPolicy-Corp and uses the _RetentionPolicyTagLinks_ parameter to associate five tags to the policy.

```PowerShell
New-RetentionPolicy "RetentionPolicy-Corp" -RetentionPolicyTagLinks "DPT-Corp-Delete","DPT-Corp-Move","DPT-Corp-Voicemail","RPT-Corp-JunkMail","Never Delete"
```

For detailed syntax and parameter information, see [New-RetentionPolicy](https://docs.microsoft.com/powershell/module/exchange/new-retentionpolicy).

## Step 3: Apply a retention policy to mailbox users

After you create a retention policy, you must apply it to mailbox users. You can apply different retention policies to different set of users. For detailed instructions, see [Apply a retention policy to mailboxes in Exchange Server](apply-retention-policies-to-mailboxes.md).

## How do you know this task worked?

After you create retention tags, add them to a retention policy, and apply the policy to a mailbox user, the next time the MRM mailbox assistant processes the mailbox, messages are moved or deleted based on settings you configured in the retention tags.

To verify that you have applied the retention policy, do the following:

1. Run the following Exchange Management Shell command to run the MRM assistant manually against a single mailbox.

   ```PowerShell
   Start-ManagedFolderAssistant -Identity <mailbox identity>
   ```

2. Log on to the mailbox using Outlook or Outlook on the web and verify that messages are deleted or moved to an archive in accordance with the policy configuration.
