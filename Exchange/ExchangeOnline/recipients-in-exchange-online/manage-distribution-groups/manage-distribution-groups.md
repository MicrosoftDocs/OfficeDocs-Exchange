---
localization_priority: Normal
description: Use the Exchange admin center (EAC) or Exchange Online PowerShell to create a new distribution group in your Exchange Online organization or to mail-enable an existing group.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c4c43493-55e1-46d2-bd4b-d6f6cecd747f
ms.reviewer: 
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Recipients.CreateDistributionGroupWizardForm.CreateDistributionGroupIntroductionWizardPage
title: Create and manage distribution groups
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create and manage distribution groups in Exchange Online

Use the Exchange admin center (EAC) or Exchange Online PowerShell to create, modify, or remove distribution groups in your Exchange Online organization.

There are two types of groups that can be used to distribute messages:

- Mail-enabled universal distribution groups (also called distribution groups) can be used only to distribute messages.

- Mail-enabled universal security groups (also called security groups) can be used to distribute messages as well as to grant access permissions to resources. For more information, see [Manage mail-enabled security groups](../../recipients-in-exchange-online/manage-mail-enabled-security-groups.md).

It's important to note the terminology differences between Active Directory and Exchange Online. In Active Directory, a distribution group refers to any group that doesn't have a security context, whether it's mail-enabled or not. In contrast, in Exchange Online, all mail-enabled groups are referred to as distribution groups, whether they have a security context or not.

## What do you need to know before you begin?

- Estimated time to complete: 2 to 5 minutes.

- To open the Exchange admin center, see [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- If your organization has configured a group naming policy, it's applied only to groups created by users. When you or other administrators use the EAC to create distribution groups, the group naming policy is ignored and isn't applied to the group name. However, if you use Exchange Online PowerShell to create or rename a distribution group, the policy is applied unless you use the _IgnoreNamingPolicy_ parameter to override the group naming policy. For more information, see:

  - [Create a distribution group naming policy](create-group-naming-policy.md)

  - [Override the distribution group naming policy](override-group-naming-policy.md)

## Use the Exchange admin center to manage distribution groups

### Use the EAC to create distribution groups

![New Try Microsoft 365 Groups](../../media/3ea82c95-9dda-450f-823b-cd0772249d81.png)

You can now create a Microsoft 365group instead of a distribution group, if you have a Microsoft 365 or Office 365 for business plan or an Exchange Online plan. Microsoft 365 groups have the features of a distribution group and much more. With Microsoft 365 groups, you can send email to a group, share a common calendar, and have a library for storing and working on group files and folders. Click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) \> **Microsoft 365 group** to get started and see [Learn about Microsoft 365 Groups](https://support.microsoft.com/office/b565caa1-5c40-40ef-9915-60fdb2d97fa2).

If you have existing distribution groups that you want to migrate to Microsoft 365 groups, see [Upgrade distribution lists to Microsoft 365 Groups in Outlook](https://docs.microsoft.com/microsoft-365/admin/manage/upgrade-distribution-lists).

If you still want to create a distribution group, use the following steps:

1. In the EAC, go to **Recipients** \> **Groups**.

2. Click **New** ![New distribution group icon](../../media/ITPro_EAC_NewDGIcon.png), and then select **Distribution group**.

3. In the **New distribution group** page that opens, configure the following settings. Settings marked with an <sup>\*</sup> are required.

   - <sup>\*</sup>**Display name**: This name appears in your organization's address book, on the To: line when email is sent to this group, and in the **Groups** list in the EAC. The display name is required, must be unique, and should be user-friendly so people recognize what it is.

   - <sup>\*</sup>**Alias**: Use this box to type the name of the alias for the group. The alias can't exceed 64 characters and must be unique. When a user types the alias in the To line of an email message, it resolves to the group's display name.

   - <sup>\*</sup>**Email address**: The email address consists of the alias on the left side of the at (@) symbol, and a domain on the right side. By default, the value of **Alias** is used for the alias value, but you can change it. For the domain value, click the drop down and select and accepted domain in your organization.

   - **Notes**: This description appears in the address book and in the Details pane in the EAC.

   - <sup>\*</sup>**Owners**: A group owner can add members to the group, approve or reject requests to join or leave the group, and approve or reject messages sent to the group. By default, the person who creates a group is the owner. All groups must have at least one owner.

     To add owners, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a recipient or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

     To remove an owner, select the owner, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).

   - **Members**: Add and remove group members and specify whether approval is required for people to join or leave the group.

     Use **Add group owners as members** to add or remove the owners as members (this setting is selected by default).

     To add members, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a recipient or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

     To remove a member, select the member, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).

   - **Choose whether owner approval is required to join the group**: Specify whether approval is required for people to join the group. Select one of the following settings:

     - **Open**: Anyone can join this group without being approved by the group owners. This is the default value.

     - **Closed**: Members can be added only by the group owners. All requests to join will be rejected automatically

     - **Owner Approval**: All requests are manually approved or rejected by the group owners. If you select this option, the group owners will receive an email message requesting approval to join the group.

   - **Choose whether the group is open to leave**: Specify whether approval is required for people to leave the group. Select one of the following settings:

     - **Open**: Anyone can leave this group without being approved by the group owners. This is the default value.

     - **Closed**: Members can be removed only by the group owners. All requests to leave will be rejected automatically.
    
For more information about using Exchange Online PowerShell to create distribution groups, see [New-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/new-distributiongroup).

4. When you're finished, click **Save** to create the distribution group.

> [!NOTE]
> By default, new distribution groups only accept messages from authenticated (internal) senders, and messages from external senders are rejected. To configure a distribution group to accept messages from all senders, you need to modify the [message delivery restriction settings](#delivery-management) for the group.
> You can create or mail-enable only universal distribution groups. To convert a domain-local or a global group to a universal group, you can use the [Set-Group](https://docs.microsoft.com/powershell/module/exchange/set-group) cmdlet using Exchange Online PowerShell. You may have mail-enabled groups that were migrated from previous versions of Exchange that are not universal groups. You can use the EAC or Exchange Online PowerShell to manage these groups

### Use the EAC to modify distribution groups

1. In the EAC, go to **Recipients** \> **Groups**.

2. In the list of groups, select the distribution group that you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_AddIcon.png).

3. On the distribution group properties page that opens, click one of the following tabs to view or change properties.

   When you're finished, click **Save**.

#### General

Use this tab to view or change basic information about the group.

- **Display name**: This name appears in the address book, on the To line when email is sent to this group, and in the **Groups list**. The display name is required and should be user-friendly so people recognize what it is. It also has to be unique in your domain.

  If you've implemented a group naming policy, the display name has to conform to the naming format defined by the policy.

- **Alias**: This is the portion of the email address that appears to the left of the at (@) symbol. If you change the alias, the primary SMTP address for the group will also be changed, and contain the new alias. Also, the email address with the previous alias will be kept as a proxy address for the group.

- **Email address**: The email address consists of the alias on the left side of the at (@) symbol, and a domain on the right side. By default, the value of **Alias** is used for the alias value, but you can change it. For the domain value, click the drop down and select and accepted domain in your organization.

- **Notes**: This description appears in the address book and in the Details pane in the EAC.

- **Hide this group from address lists**: Select this check box if you don't want users to see this group in the address book. To send email to this group, a sender has to type the group's alias or email address on the To: or Cc: lines.

#### Ownership

Use this tab to assign group owners. The group owner can add members to the group, approve or reject requests to join or leave the group, and approve or reject messages sent to the group. By default, the person who creates a group is the owner. All groups must have at least one owner.

To add owners, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a recipient, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

To remove an owner, select the owner, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).

#### Membership

Use this tab to add or remove group members. Group owners don't need to be members of the group.

To add members, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a recipient or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

To remove a member, select the member, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).

#### Membership approval

Use this tab to specify whether approval is required for users to join or leave the group.

- **Choose whether owner approval is required to join the group**: Select one of the following settings:

  - **Open**: Anyone can join this group without being approved by the group owners. This is the default value.

  - **Closed**: Members can be added only by the group owners. All requests to join will be rejected automatically

  - **Owner Approval**: All requests are manually approved or rejected by the group owners. If you select this option, the group owners will receive an email message requesting approval to join the group.

- **Choose whether the group is open to leave**: Specify whether approval is required for people to leave the group. Select one of the following settings:

  - **Open**: Anyone can leave this group without being approved by the group owners. This is the default value.

  - **Closed**: Members can be removed only by the group owners. All requests to leave will be rejected automatically.

#### Delivery management

Use this tab to manage who can send email to this group.

- **Only senders inside my organization**: Only internal (authenticated) senders are allowed to send messages to this group. Messages from external senders are rejected. This is the default setting.

- **Senders inside and outside of my organization**: Allow anyone to send messages to the group.

  You can configure the group to accept messages only from specific senders.
  
  To add senders, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a sender or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

  To remove a sender from the list, select the sender, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).
  
  > [!IMPORTANT]
  > Mail contacts are always considered external users. So, if you configure the group to only accept messages from internal senders and you add mail contacts to the list of allowed senders, messages from those mail contacts are still rejected.

#### Message approval

Use this tab to set options for moderating the group. Moderators approve or reject messages sent to the group before they reach the group members.

- **Messages sent to this group need to be approved by a moderator**: When you enable moderation for the group, messages sent to the group require approval by a specified moderator or a group owner before the message is delivered to the group members. This setting is disabled by default.

- **Group moderators**: To add moderators, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a recipient, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

  To remove a moderator, select the moderator, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).

  If you enable moderation for the group but don't specify any moderators, group owners are responsible for approving messages that are sent to the group.

- **Senders who don't require message approval**: Messages sent to the group by the specified senders don't require approval from a moderator.

  To add senders, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a sender or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

  To remove senders, select the senders, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).

- **Select moderation notifications**: Configure the sender notification options for message approval:

  - **Notify all senders when their messages aren't approved**: Internal and external senders are notified when their messages aren't approved. This is the default value.

  - **Notify senders in your organization when their messages aren't approved**: Internal senders are notified when their messages aren't approved. External senders aren't notified.

  - **Don't notify anyone when a message isn't approved**: No notification messages are sent.

#### Email options

Use the **Email addresses** tab to view or change the email addresses associated with the group. This includes the group's primary SMTP address and any associated proxy addresses. The primary SMTP address (also known as the reply address) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

- **Add**: Click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **New email address** page that appears, configure the following settings:

  - **Email address type**: Verify **SMTP** is selected.
  - **Email address**: Enter the email address to add.
  - **Make this the reply address**

  When you're finished, click **OK**.

- **Edit**: Select the email address that you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png). In the **Email address** page that appears, configure the following settings:

  - **Email address**: Modify the existing email address.
  - **Make this the reply address**: This setting only appears if the email address you selected isn't already the reply address.

  When you're finished, click **OK**.

- **Remove**: Select the email address that you want to remove, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.gif). You can't remove the reply address.

#### MailTip

Use the **MailTip** tab to add an alert for potential issues before a user sends messages to this recipient. The text is displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc lines of a new email message.

MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

#### Group delegation

Use this section to assign permissions to a user (called a delegate) to allow them to send messages as the group or send messages on behalf of the group. You can assign the following permissions:

- **Send As**: Allows the delegate to send messages as if they came directly from the group. There's no indication that the message was sent by the delegate. After this permission is assigned, the delegate has the option to add the group to the From: line to indicate that the message was sent by the group.

- **Send on Behalf**: Allows the delegate to send messages from the group. The From address of these messages clearly shows that the message was sent by the delegate ("_\<Delegate\>_ on behalf of _\<Group\>_"). However, replies to these messages are sent to the group, not to the delegate. After this permission is assigned, the delegate has the option to add the group in the From: line.

To add senders, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the dialog that appears, find and select a sender or group, and then click **add ->**. Repeat this step as many times as necessary. When you're finished, click **OK**.

To remove a sender from the list, select the sender, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).

### Use the EAC to remove distribution groups

1. In the EAC, go to **Recipients** \> **Groups**.

2. In the list of groups, select the distribution group that you want to remove, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.gif).

## Use PowerShell to manage distribution groups

### Use Exchange Online PowerShell to create distribution groups

This example creates a distribution group with an alias **itadmin** and the name **IT Administrators**. The distribution group is created in the default OU, and anyone can join this group without approval by the group owners.

```PowerShell
New-DistributionGroup -Name "IT Administrators" -Alias itadmin -MemberJoinRestriction open
```

For detailed syntax and parameter information, see [New-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/new-distributiongroup).

### Use Exchange Online PowerShell to modify distribution groups

Use the **Get-DistributionGroup** and **Set-DistributionGroup** cmdlets to view and change properties for distribution groups. Advantages of using Exchange Online PowerShell are the ability to change the properties that aren't available in the EAC and to change properties for multiple groups. For information about which parameters correspond to distribution group properties, see the following topics:

- [Get-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/get-distributiongroup)

- [Set-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-distributiongroup)

Here are some examples of using Exchange Online PowerShell to change distribution group properties.

This example changes the primary SMTP address (also called the reply address) for the Seattle Employees distribution group from employees@contoso.com to sea.employees@contoso.com. Also, the previous reply address will be kept as a proxy address.

```PowerShell
Set-DistributionGroup "Seattle Employees" -EmailAddresses SMTP:sea.employees@contoso.com,smtp:employees@contoso.com
```

This example enables moderation for the distribution group Customer Support and sets the moderator to Amy. In addition, this moderated distribution group will notify senders who send mail from within the organization if their messages aren't approved.

```PowerShell
Set-DistributionGroup -Identity "Customer Support" -ModeratedBy "Amy" -ModerationEnabled $true -SendModerationNotifications 'Internal'
```

This example changes the user-created distribution group Dog Lovers to require the group manager to approve users' requests to join the group. In addition, by using the _BypassSecurityGroupManagerCheck_ parameter, the group manager will not be notified that a change was made to the distribution group's settings.

```PowerShell
Set-DistributionGroup -Identity "Dog Lovers" -MemberJoinRestriction 'ApprovalRequired' -BypassSecurityGroupManagerCheck
```

## How do you know these procedures worked?

To verify that you've successfully created, modified, or removed a distribution group, do any of the following steps:

- In the EAC, go to **Recipients** \> **Groups**. Verify that the group is listed (or not listed). The **Group Type** is **Distribution group**. Select the group and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) to verify the property settings.

- In Exchange Online PowerShell, replace \<GroupIdentity\> with the name, alias, or email address of the distribution group, and run the following command to verify the settings:

  ```powershell
  Get-DistributionGroup -Identity "<GroupIdentity>" | Format-List
  ```

  To view specific properties, run the following command:

  ```PowerShell
  Get-DistributionGroup -Identity "<GroupIdentity>" | Format-List Name,PrimarySmtpAddress
  ```

- To get a list of members in the group, replace \<GroupIdentity\> with the name, alias, or email address of the distribution group, and run the following command:

  ```PowerShell
  Get-DistributionGroupMember -Identity "<GroupIdentity>"
  ```

  For detailed syntax and parameter information, see [Get-DistributionGroupMember](https://docs.microsoft.com/powershell/module/exchange/get-distributiongroupmember).
