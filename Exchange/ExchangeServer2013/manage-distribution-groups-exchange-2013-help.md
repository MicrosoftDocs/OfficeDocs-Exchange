---
title: 'Create and manage distribution groups: Exchange 2013 Help'
TOCTitle: Create and manage distribution groups
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.custom:
- 'Microsoft.Exchange.Management.SnapIn.Esm.Recipients.CreateDistributionGroupWizardForm.CreateDistributionGroupIntroductionWizardPage'
ms.assetid: c4c43493-55e1-46d2-bd4b-d6f6cecd747f
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
---

# Create and manage distribution groups in Exchange 2013

_**Applies to:** Exchange Server 2013_

Use the Exchange admin center (EAC) or Exchange Management Shell to create a new distribution group in your Exchange organization or to mail-enable an existing group in Active Directory.

There are two types of groups that can be used to distribute messages:

- Mail-enabled universal distribution groups (also called distribution groups) can be used only to distribute messages.

- Mail-enabled universal security groups (also called security groups) can be used to distribute messages as well as to grant access permissions to resources in Active Directory. For more information, see [Manage mail-enabled security groups](manage-mail-enabled-security-groups-exchange-2013-help.md).

It's important to note the terminology differences between Active Directory and Exchange. In Active Directory, a distribution group refers to any group that doesn't have a security context, whether it's mail-enabled or not. In contrast, in Exchange, all mail-enabled groups are referred to as distribution groups, whether they have a security context or not.

## What do you need to know before you begin?

- Estimated time to complete: 2 to 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Distribution groups" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- If your organization has configured a group naming policy, it's applied only to groups created by users. When you or other administrators use the EAC to create distribution groups, the group naming policy is ignored and isn't applied to the group name. However, if you use the Shell to create or rename a distribution group, the policy is applied unless you use the _IgnoreNamingPolicy_ parameter to override the group naming policy. For more information, see:

  - [Create a distribution group naming policy](create-group-naming-policy-exchange-2013-help.md)

  - [Override the distribution group naming policy](override-group-naming-policy-exchange-2013-help.md)

## Create a distribution group

### Use the EAC to create a distribution group

1. In the EAC, navigate to **Recipients** \> **Groups**.

2. Click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif) \> **Distribution group**.

3. Click or tap the **New distribution group** wizard.

4. On the **New distribution group** page, complete the following boxes:

   - **\* Display name**: Use this box to type the display name. This name appears in your organization's address book, on the To: line when email is sent to this group, and in the Groups list in the EAC. The display name is required and should be user-friendly so people recognize what it is. It also must be unique in the forest.

   - **\* Alias**: Use this box to type the name of the alias for the group. The alias can't exceed 64 characters and must be unique in the forest. When a user types the alias in the To: line of an email message, it resolves to the group's display name.

   - **Organizational unit**: (You'll only see this option in Exchange 2013 on-premises) You can select an organizational unit (OU) other than the default (which is the recipient scope). If the recipient scope is set to the forest, the default value is set to the Users container in the Active Directory domain that contains the computer on which the EAC is running. If the recipient scope is set to a specific domain, the Users container in that domain is selected by default. If the recipient scope is set to a specific OU, that OU is selected by default.

     To select a different OU, click **Browse**. The dialog box displays all OUs in the forest that are within the specified scope. Select the OU you want, and then click **OK**.

   - **\* Owners**: By default, the person who creates a group is the owner. All groups must have at least one owner. You can add owners by clicking **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

   - **Members**: Use this section to add members and to specify whether approval is required for people to join or leave the group.

     Group owners don't have to be members of the group. Use **Add group owners as members** to add or remove the owners as members.

     To add members to the group, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). When you've finished adding members, click **OK** to return to the **New distribution group** page.

     Under **Choose whether owner approval is required to join the group**, specify whether approval is required for people to join the group. Select one of the following settings:

     - **Open: Anyone can join this group without being approved by the group owners**: This is the default setting.

     - **Closed: Members can be added only by the group owners. All requests to join will be rejected automatically**

     - **Owner Approval: All requests are manually approved or rejected by the group owners**: If you select this option, the group owner or owners will receive an email message requesting approval to join the group.

     Under **Choose whether the group is open to leave**, specify whether approval is required for people to leave the group. Select one of the following settings:

     - **Open: Anyone can leave this group without being approved by the group owners**: This is the default setting.

     - **Closed: Members can be removed only by the group owners. All requests to leave will be rejected automatically**

5. When you've finished, click **Save** to create the distribution group.

> [!NOTE]
> By default, new distribution groups require that all senders be authenticated. This prevents external senders from sending messages to distribution groups. To configure a distribution group to accept messages from all senders, you must modify the message delivery restriction settings for that distribution group.

### Use the Shell to create a distribution group

This example creates a distribution group with an alias **itadmin** and the name **IT Administrators**. The distribution group is created in the default OU, and anyone can join this group without approval by the group owners.

```powershell
New-DistributionGroup -Name "IT Administrators" -Alias itadmin -MemberJoinRestriction open
```

For more information about using the Shell to create distribution groups, see [New-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/new-distributiongroup).

### How do you know this worked?

To verify that you've successfully created a distribution group, do one of the following:

- In the EAC, navigate to **Recipients** \> **Groups**. The new distribution group is displayed in the group list. Under **Group Type**, the type is **Distribution group**.

- In the Shell, run the following command to display information about the new distribution group.

  ```powershell
  Get-DistributionGroup <Name> | Format-List Name,RecipientTypeDetails,PrimarySmtpAddress
  ```

> [!NOTE]
> You can create or mail-enable only universal distribution groups. To convert a domain-local or a global group to a universal group, you can use the [Set-Group](https://docs.microsoft.com/powershell/module/exchange/set-group) cmdlet using the Shell. You may have mail-enabled groups that were migrated from previous versions of Exchange that are not universal groups. You can use the EAC or the Shell to manage these groups

## Change distribution group properties

### Use the EAC to change distribution group properties

1. In the EAC, navigate to **Recipients** \> **Groups**.

2. In the list of groups, click the distribution group that you want to view or change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the group properties page, click one of the following sections to view or change properties.

### General

Use this section to view or change basic information about the group.

- **\* Display name**: This name appears in the address book, on the To: line when email is sent to this group, and in the Groups list. The display name is required and should be user-friendly so people recognize what it is. It also has to be unique in your domain.

  If you've implemented a group naming policy, the display name has to conform to the naming format defined by the policy.

- **\* Alias**: This is the portion of the email address that appears to the left of the at (@) symbol. If you change the alias, the primary SMTP address for the group will also be changed, and contain the new alias. Also, the email address with the previous alias will be kept as a proxy address for the group.

- **Description**: Use this box to describe the group so people know what the purpose of the group is. This description appears in the address book and in the Details pane in the EAC.

- **Hide this group from address lists**: Select this check box if you don't want users to see this group in the address book. To send email to this group, a sender has to type the group's alias or email address on the To: or Cc: lines.

   > [!TIP]
   > Consider hiding security groups because they're typically used to assign permissions to group members and not to send email.

- **Organizational unit**: This read-only box displays the organizational unit (OU) that contains the distribution group. You have to use Active Directory Users and Computers to move the group to a different OU.

### Ownership

Use this section to assign group owners. The group owner can add members to the group, approve or reject requests to join or leave the group, and approve or reject messages sent to the group. By default, the person who creates a group is the owner. All groups must have at least one owner.

You can add owners by clicking **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). You can remove an owner by selecting the owner and then clicking **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

### Membership

Use this section to add or remove members. Group owners don't have to be members of the group. Under **Members**, you can add members by clicking **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). You can remove a member by selecting a user in the member list and then clicking **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

### Membership approval

Use this section to specify whether approval is required for users to join or leave the group.

- **Choose whether owner approval is required to join the group** Select one of the following settings:

  - **Open: Anyone can join this group without being approved by the group owners**

  - **Closed: Members can be added only by the group owners. All requests to join will be rejected automatically**

  - **Owner Approval: All requests are approved or rejected by the group owners**: If you select this option, the group owner or owners receive an email requesting approval to join the group.

  - **Choose whether the group is open to leave** Select one of the following settings:

    - **Open: Anyone can leave this group without being approved by the group owners**

    - **Closed: Members can be removed only by the group owners. All requests to leave will be rejected automatically**

### Delivery management

Use this section to manage who can send email to this group.

- **Only senders inside my organization**: Select this option to allow only senders in your organization to send messages to the group. This means that if someone outside of your organization sends an email message to this group, it will be rejected. This is the default setting.

- **Senders inside and outside of my organization**: Select this option to allow anyone to send messages to the group.

  You can further limit who can send messages to the group by allowing only specific senders to send messages to this group. Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) and then select one or more recipients. If you add senders to this list, they are the only ones who can send mail to the group. Mail sent by anyone not in the list will be rejected.

  To remove a person or a group from the list, select them in the list and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

   > [!IMPORTANT]
   > If you've configured the group to allow only senders inside your organization to send messages to the group, email sent from a mail contact will be rejected, even if they are added to this list.

### Message approval

Use this section to set options for moderating the group. Moderators approve or reject messages sent to the group before they reach the group members.

- **Messages sent to this group have to be approved by a moderator**: This check box isn't selected by default. If you select this check box, incoming messages are reviewed by the group moderators before delivery. Group moderators can approve or reject incoming messages.

- **Group moderators**: To add group moderators, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). To remove a moderator, select the moderator, and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif). If you've selected "Messages sent to this group have to be approved by a moderator" and you don't select a moderator, messages to the group are sent to the group owners for approval.

- **Senders who don't require message approval**: To add people or groups that can bypass moderation for this group, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). To remove a person or a group, select the item, and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

- **Select moderation notifications**: Use this section to set how users are notified about message approval.

  - **Notify all senders when their messages aren't approved**: This is the default setting. Notify all senders, inside and outside your organization, when their message isn't approved.

  - **Notify senders in your organization when their messages aren't approved**: When you select this option, only people or groups in your organization are notified when a message that they sent to the group isn't approved by a moderator.

  - **Don't notify anyone when a message isn't approved**: When you select this option, notifications aren't sent to message senders whose messages aren't approved by the group moderators.

### Email options

Use this section to view or change the email addresses associated with the group. This includes the group's primary SMTP addresses and any associated proxy addresses. The primary SMTP address (also known as the reply address) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

- **Add**: Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) to add a new email address for this mailbox. Select one of following address types:

  - **SMTP**: This is the default address type. Click this button and then type the new SMTP address in the **\* Email address** box.

    > [!NOTE]
    > To make the new address the primary SMTP address for the group, select the **Make this the reply address** check box.

  - **Custom address type**: Click this button and type one of the supported non-SMTP email address types in the **\* Email address** box.

    > [!NOTE]
    > With the exception of X.400 addresses, Exchange doesn't validate custom addresses for correct formatting. You must make sure that the custom address you specify complies with the format requirements for that address type.

- **Edit**: To change an email address associated with the group, select it in the list, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

    > [!NOTE]
    > To make an existing address the primary SMTP address for the group, select the **Make this the reply address** check box.

- **Remove**: To delete an email address associated with the group, select it in the list, and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

- **Automatically update email addresses based on the email address policy applied to this recipient**: Select this check box to have the recipient's email addresses automatically updated based on changes made to email address policies in your organization. This box is selected by default.

### MailTip

Use this section to add a MailTip to alert users of potential issues if they send a message to this group. A MailTip is text that's displayed in the InfoBar when this group is added to the To, Cc, or Bcc lines of a new email message. For example, you could add a MailTip to large groups to warn potential senders that their message will be sent to lots of people.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

### Group delegation

Use this section to assign permissions to a user (called a delegate) to allow them to send messages as the group or send messages on behalf of the group. You can assign the following permissions:

- **Send As**: This permission allows the delegate to send messages as the group. After this permission is assigned, the delegate has the option to add the group to the **From** line to indicate that the message was sent by the group.

- **Send on Behalf**: This permission also allows a delegate to send messages on behalf of the group. After this permission is assigned, the delegate has the option to add the group in the **From** line. The message will appear to be sent by the group and will say that it was sent by the delegate on behalf of the group.

To assign permissions to delegates, click **Add** under the appropriate permission to display the **Select Recipient** page, which displays a list of all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search**.

### Use the Shell to change distribution group properties

Use the **Get-DistributionGroup** and **Set-DistributionGroup** cmdlets to view and change properties for distribution groups. Advantages of using the Shell are the ability to change the properties that aren't available in the EAC and to change properties for multiple groups. For information about which parameters correspond to distribution group properties, see the following topics:

- [Get-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/get-distributiongroup)

- [Set-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-distributiongroup)

Here are some examples of using the Shell to change distribution group properties.

This example changes the primary SMTP address (also called the reply address) for the Seattle Employees distribution group from employees@contoso.com to sea.employees@contoso.com. Also, the previous reply address will be kept as a proxy address.

```powershell
Set-DistributionGroup "Seattle Employees" -EmailAddresses SMTP:sea.employees@contoso.com,smtp:employees@contoso.com
```

This example limits the maximum message size that can be sent to all distribution groups in the organization to 10 megabytes (MB).

```powershell
Get-DistributionGroup -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'MailUniversalDistributionGroup'" | Set-DistributionGroup -MaxReceiveSize 10MB
```

This example enables moderation for the distribution group Customer Support and sets the moderator to Amy. In addition, this moderated distribution group will notify senders who send mail from within the organization if their messages aren't approved.

```powershell
Set-DistributionGroup -Identity "Customer Support" -ModeratedBy "Amy" -ModerationEnabled $true -SendModerationNotifications 'Internal'
```

This example changes the user-created distribution group Dog Lovers to require the group manager to approve users' requests to join the group. In addition, by using the _BypassSecurityGroupManagerCheck_ parameter, the group manager will not be notified that a change was made to the distribution group's settings.

```powershell
Set-DistributionGroup -Identity "Dog Lovers" -MemberJoinRestriction 'ApprovalRequired' -BypassSecurityGroupManagerCheck
```

### How do you know this worked?

To verify that you've successfully changed properties for a distribution group, do the following:

- In the EAC, select the group and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif) to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the Details pane for the selected group.

- In the Shell, use the **Get-DistributionGroup** cmdlet to verify the changes. One advantage of using the Shell is that you can view multiple properties for multiple groups. In the example above where the recipient limit was changed, run the following command to verify the new value.

  ```powershell
  Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Format-List Name,RecipientLimits
  ```

  For the example above where the message limits were changed, run this command.

  ```powershell
  Get-Mailbox -OrganizationalUnit "Marketing" | Format-List Name,IssueWarningQuota,ProhibitSendQuota,ProhibitSendReceiveQuota,UseDatabaseQuotaDefaults
  ```
