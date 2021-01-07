---
localization_priority: Normal
description: Dynamic distribution groups are mail-enabled Active Directory group objects that are created to expedite the mass sending of email messages and other information within a Microsoft Exchange organization.
ms.topic: article
author: msdmaguire
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Recipients.CreateDynamicGroupWizardForm.CreateDynamicGroupInformationWizardPage
ms.author: dmaguire
ms.assetid: 8ef85d0a-41df-4b5c-b8e7-ca8d09c048ca
ms.reviewer: 
title: Manage dynamic distribution group
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage dynamic distribution groups

Dynamic distribution groups are mail-enabled Active Directory group objects that are created to expedite the mass sending of email messages and other information within a Microsoft Exchange organization.

Unlike regular distribution groups that contain a defined set of members, the membership list for dynamic distribution groups is calculated each time a message is sent to the group, based on the filters and conditions that you define. When an email message is sent to a dynamic distribution group, it's delivered to all recipients in the organization that match the criteria defined for that group.

> [!IMPORTANT]
> A dynamic distribution group includes any recipient in Active Directory with attribute values that match its filter. If a recipient's properties are modified to match the filter, the recipient could inadvertently become a group member and start receiving messages that are sent to the group. Well-defined, consistent account provisioning processes will reduce the chances of this issue occurring.

## What do you need to know before you begin?

- Estimated time to complete: 2 to 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://docs.microsoft.com/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Create a dynamic distribution group

### Use the new EAC to create a dynamic distribution group

1. In the new EAC, navigate to **Recipients** > **Groups**.

2. Click **Add a group** and follow the instructions in the details pane.

   - Under **Choose a group type** section, select **Dynamic distribution group** and click **Next**.
   
   - Under **Set up the basics** section, enter the details and click **Next**.
   
3. In **Assign Users** section, select the group owner from the drop-down list.

4. Use the **Members** section to specify the types of recipients for the group and set up rules that will determine membership. Select one of the following boxes:

   - **All recipient types**: Choose this option to send messages that meet the criteria defined for this group to all recipient types.

   - **Only the following recipient types**: Messages that meet the criteria defined for this group will be sent to one or more of the following recipient types:

     - **Users with Exchange mailboxes**: Select this check box if you want to include users that have Exchange mailboxes. Users that have Exchange mailboxes are those that have a user domain account and a mailbox in the Exchange organization.

     - **Mail users with external email addresses**: Select this check box if you want to include users that have external email addresses. Users that have external email accounts have user domain accounts in Active Directory, but use email accounts that are external to the organization. This enables them to be included in the global address list (GAL) and added to distribution lists.

     - **Resource mailboxes**: Select this check box if you want to include Exchange resource mailboxes. Resource mailboxes allow you to administer company resources through a mailbox, such as a conference room or a company vehicle.

     - **Mail contacts with external email addresses**: Select this check box if you want to include contacts that have external email addresses. Contacts that have external email addresses don't have user domain accounts in Active Directory, but the external email address is available in the GAL.

     - **Mail-enabled groups**: Select this check box if you want to include security groups or distribution groups that have been mail-enabled. Mail-enabled groups are similar to distribution groups. Email messages that are sent to a mail-enabled group account will be delivered to several recipients.

5. Select one of the following attributes from the drop-down list and provide a value to define the criteria for membership in this group.

   |**Attribute**|**Send message to a recipient if...**|
   |:-----|:-----|
   |**State or province**|The specified value matches the recipient's State or province property.|
   |**Company**|The specified value matches the recipient's Company property.|
   |**Department**|The specified value matches the recipient's Department property.|
   |**Custom attribute N** (where N is a number from 1 to 15)|The specified value matches the recipient's CustomAttributeN property.|

    > [!IMPORTANT]
    > The values that you enter for the selected attribute must exactly match those that appear in the recipient's properties. For example, if you enter **Washington** for **State or province**, but the value for the recipient's property is **WA**, the condition will not be met. Also, text-based values that you specify aren't case-sensitive. For example, if you specify **Contoso** for the **Company** attribute, messages will be sent to a recipient if this value is **contoso**.

6. To add another rule to define the criteria for membership, click **Add another rule**, when you've finished, click **Next**.

   > [!IMPORTANT]
   > If you add multiple rules to define membership, a recipient must meet the criteria of each rule to receive a message sent to the group. In other words, each rule is connected with the Boolean operator **AND**.

7. In **Edit settings** section, enter the group email address and click **Next**.

8. In **Review and finish adding group** section, verify all the details, click **Create group** and then click **Close**.

> [!NOTE]
> If you want to specify rules for attributes other than the ones available in the new EAC, you must use Exchange Online PowerShell to create a dynamic distribution group. Keep in mind that the filter and condition settings for dynamic distribution groups that have custom recipient filters can be managed only by using Exchange Online PowerShell. For an example of how to create a dynamic distribution group with a custom query, see the next section on using Exchange Online PowerShell to create a dynamic distribution group.

### Use the Classic EAC to create a dynamic distribution group

1. In the Classic EAC, navigate to **Recipients** \> **Groups** \> **New** \> **Dynamic distribution group**.

2. On the **New dynamic distribution group** page, complete the following boxes:

   - **Display name**: Use this box to type the display name. This name appears in the shared address book, on the To: line when email is sent to this group, and in the Groups list in the Classic EAC. The display name is required and should be user-friendly so people recognize what it is. It also must be unique in the forest.
   
   > [!NOTE]
   > Group naming policy isn't applied to dynamic distribution groups.
   
   - **Alias**: Use this box to type the name of the alias for the group. The alias cannot exceed 64 characters and must be unique in the forest. When a user types the alias in the To: line of an email message, it resolves to the group's display name.
   
   - **Description**: Use this box to describe the group so people know what the purpose of the group is. This description appears in the shared address book.
   
   - **Organizational unit**: You can select an organizational unit (OU) other than the default (which is the recipient scope). If the recipient scope is set to the forest, the default value is set to the Users container in the Active Directory domain that contains the computer on which the Classic EAC is running. If the recipient scope is set to a specific domain, the Users container in that domain is selected by default. If the recipient scope is set to a specific OU, that OU is selected by default.
   
   To select a different OU, click **Browse**. The dialog box displays all OUs in the forest that are within the specified scope. Select the OU you want, and then click **OK**.
   
   - **Owner**: An owner for a dynamic distribution group is optional. You can add owners by clicking **Browse** and then selecting users from the list.
   
3. Use the **Members** section to specify the types of recipients for the group and set up rules that will determine membership. Select one of the following boxes:

   - **All recipient types**: Choose this option to send messages that meet the criteria defined for this group to all recipient types.

   - **Only the following recipient types**: Messages that meet the criteria defined for this group will be sent to one or more of the following recipient types:

   - **Users with Exchange mailboxes**: Select this check box if you want to include users that have Exchange mailboxes. Users that have Exchange mailboxes are those that have a user domain account and a mailbox in the Exchange organization.

   - **Users with external email addresses**: Select this check box if you want to include users that have external email addresses. Users that have external email accounts have user domain accounts in Active Directory, but use email accounts that are external to the organization. This enables them to be included in the global address list (GAL) and added to distribution lists.

   - **Resource mailboxes**: Select this check box if you want to include Exchange resource mailboxes. Resource mailboxes allow you to administer company resources through a mailbox, such as a conference room or a company vehicle.

   - **Contacts with external email addresses**: Select this check box if you want to include contacts that have external email addresses. Contacts that have external email addresses don't have user domain accounts in Active Directory, but the external email address is available in the GAL.

   - **Mail-enabled groups**: Select this check box if you want to include security groups or distribution groups that have been mail-enabled. Mail-enabled groups are similar to distribution groups. Email messages that are sent to a mail-enabled group account will be delivered to several recipients.

4. Click **Add a rule** to define the criteria for membership in this group.

5. Select one of the following recipient attributes from the drop-down list and provide a value. If the value for the selected attribute matches that value you define, the recipient receives a message sent to this group.

   |**Attribute**|**Send message to a recipient if...**|
   |:-----|:-----|
   |**Recipient container**|The recipient object resides in the specified domain or OU.|
   |**State or province**|The specified value matches the recipient's State or province property.|
   |**Company**|The specified value matches the recipient's Company property.|
   |**Department**|The specified value matches the recipient's Department property.|
   |**Custom attribute N** (where N is a number from 1 to 15)|The specified value matches the recipient's CustomAttributeN property.|

    > [!IMPORTANT]
    > The values that you enter for the selected attribute must exactly match those that appear in the recipient's properties. For example, if you enter **Washington** for **State or province**, but the value for the recipient's property is **WA**, the condition will not be met. Also, text-based values that you specify aren't case-sensitive. For example, if you specify **Contoso** for the **Company** attribute, messages will be sent to a recipient if this value is **contoso**.

6. In the **Specify words or phrases** window, type the value in the text box. Click **Add** and then click **OK**.

7. To add another rule to define the criteria for membership, click **Add a rule** under the previous rule that you created.

   > [!IMPORTANT]
   > If you add multiple rules to define membership, a recipient must meet the criteria of each rule to receive a message sent to the group. In other words, each rule is connected with the Boolean operator **AND**.

8. When you've finished, click **Save** to create the dynamic distribution group.

> [!NOTE]
> If you want to specify rules for attributes other than the ones available in the Classic EAC, you must use Exchange Online PowerShell to create a dynamic distribution group. Keep in mind that the filter and condition settings for dynamic distribution groups that have custom recipient filters can be managed only by using Exchange Online PowerShell. For an example of how to create a dynamic distribution group with a custom query, see the next section on using Exchange Online PowerShell to create a dynamic distribution group.

### Use Exchange Online PowerShell to create a dynamic distribution group

This example creates the dynamic distribution group "Mailbox Users DDG" that contains only mailbox users.

```PowerShell
New-DynamicDistributionGroup -IncludedRecipients MailboxUsers -Name "Mailbox Users DDG" -OrganizationalUnit Users
```

This example creates a dynamic distribution group with a custom recipient filter. The dynamic distribution group contains all mailbox users on a server called Server1.

```PowerShell
New-DynamicDistributionGroup -Name "Mailbox Users on Server1" -OrganizationalUnit Users -RecipientFilter "(RecipientTypeDetails -eq 'UserMailbox') -and (ServerName -eq 'Server1')"
```

This example creates a dynamic distribution group with a custom recipient filter. The dynamic distribution group contains all mailbox users that have a value of "FullTimeEmployee" in the **CustomAttribute10** property.

```PowerShell
New-DynamicDistributionGroup -Name "Full Time Employees" -RecipientFilter "(RecipientTypeDetails -eq 'UserMailbox') -and (CustomAttribute10 -eq 'FullTimeEmployee')"
```

For detailed syntax and parameter information, see [New-DynamicDistributionGroup](https://docs.microsoft.com/powershell/module/exchange/new-dynamicdistributiongroup).

### How do you know this worked?

To verify that you've successfully created a dynamic distribution group, do one of the following:

- In the new EAC, navigate to **Recipients** \> **Groups**. The new dynamic distribution group is displayed in the group list. Under **Type**, the type is **Dynamic distribution group**.

- In the Classic EAC, navigate to **Recipients** \> **Groups**. The new dynamic distribution group is displayed in the group list. Under **Group Type**, the type is **Dynamic distribution group**.

- In Exchange Online PowerShell, run the following command to display information about the new dynamic distribution group.

  ```PowerShell
  Get-DynamicDistributionGroup | Format-List Name,RecipientTypeDetails,RecipientFilter,PrimarySmtpAddress
  ```
## Change dynamic distribution group properties

### Use the new EAC to change dynamic distribution group properties

1. In the new EAC, navigate to **Recipients** > **Groups** > **Dynamic distribution list**.

2. In the list of groups, click the dynamic distribution group that you want to view or change.

3. On the group's properties page, click one of the following sections to view or change properties.

#### General

Use this section to view or change basic information about the group.

- **Name**: This name appears in the address book, on the To: line when email is sent to this group, and in the Groups list. The display name is required and should be user-friendly so people recognize what it is. It also has to be unique in your domain.

- **Description**: Use this box to describe the group so people know what the purpose of the group is. This description appears in the address book and in the Details pane in the new EAC.

#### Email options in new EAC

Use this section to view or change the email addresses associated with the group. This includes the group's primary SMTP addresses and any associated proxy addresses. Under **Edit email addresses** page, change/edit the **Primary email address**, add/delete **Aliases** and then click **Save changes**. 

You can also select the group and then click **Edit email addresses** from the toolbar to change/edit the **Primary email address**, add/delete **Aliases** and then click **Save changes**.

#### Members

Use this section to change/edit the following:

- Under **Owners** section, click **View all and manage owners** to add/remove group owners from the drop-down list and then click **Save changes**. The dynamic distribution group must have at least one owner. 

- Use **Members** section to change the criteria used to determine membership of the group. You can delete or change existing membership rules and add new rules. For procedures that tell you how to do this, see [Use the new EAC to create a dynamic distribution group](#use-the-new-eac-to-create-a-dynamic-distribution-group).

#### Settings

Under **General settings** section, select the checkbox **Hide from my organization's global address list** if you want to hide the group from the list.

#### Delivery management

Use this section to manage who can send email to this group.

- **Sender options**

  By default, only people inside your organization can send message to this group. You can also allow people outside the organization to send to this group.
 
  - **Only allow messages from people inside my organization**: Select this option to allow only senders in your organization to send messages to the group. This means that if someone outside your organization sends an email message to this group, it is rejected. This is the default setting.

  - **Allow messages from people inside and outside my organization**: Select this option to allow anyone to send messages to the group.

- **Specified senders**

  You can further limit who can send messages to the group by allowing only specific senders to send messages to this group. Select/remove one or more recipients/group from the drop-down list. If you add senders to this list, they are the only ones who can send mail to the group. Mail sent by anyone not in the list will be rejected.

  > [!IMPORTANT]
  > If you've configured the group to allow only senders inside your organization to send messages to the group, email sent from a mail contact is rejected, even if they're added to this list.

#### Manage delegates

Use this section to assign permissions to a user (called a delegate) to allow them to send messages as the group or send messages on behalf of the group. You can assign the following permissions:

- **Send As**: This permission allows the delegate to send messages as the group. After this permission is assigned, the delegate has the option to add the group to the **From** line to indicate that the message was sent by the group.

- **Send on Behalf**: This permission also allows a delegate to send messages on behalf of the group. After this permission is assigned, the delegate has the option to add the group on the **From** line. The message will appear to be sent by the group and will say that it was sent by the delegate on behalf of the group.

To assign permissions to delegates in new EAC, add the delegates under the **Edit delegates** page, select the **Permission type** from the drop-down list and click **Save changes**.

#### Message approval 

Use this section to set options for moderating the group. Moderators approve or reject messages sent to the group before they reach the group members.

- **Require moderator approval for messages sent to this group**: This check box isn't selected by default. If you select this check box, incoming messages are reviewed by the group moderators before delivery. Group moderators can approve or reject incoming messages.

- **Group moderators**: To add/remove group moderators, search/add users from the drop-down list. If you've selected **Require moderator approval for messages sent to this group** and you don't select a moderator, messages to the group are sent to the group owners for approval.

- **Add senders who don't require message approval**: To add/remove users that can bypass moderation for this group, search/add users from the drop-down list.

- **Notify a sender if their message isn't approved:**: Use this section to set how users are notified about message approval.

  - **Only sender**: This is the default setting. Notify all senders, inside and outside your organization, when their message isn't approved.

  - **Only senders in your organization**: When you select this option, only users or groups in your organization are notified when a message that they sent to the group isn't approved by a moderator.

  - **No notifications**: When you select this option, notifications aren't sent to senders whose messages aren't approved by the group moderators.

### Use the Classic EAC to change dynamic distribution group properties

1. In the Classic EAC, navigate to **Recipients** \> **Groups**.

2. In the list of groups, click the dynamic distribution group that you want to view or change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the group's properties page, click one of the following sections to view or change properties.

#### General

Use this section to view or change basic information about the group.

- **Display name**: This name appears in the address book, on the To: line when email is sent to this group, and in the Groups list. The display name is required and should be user-friendly so people recognize what it is. It also has to be unique in your domain.

- **Alias**: This is the portion of the email address that appears to the left of the at (@) symbol. If you change the alias, the primary SMTP address for the group will also be changed, and contain the new alias. Also, the email address with the previous alias will be kept as a proxy address for the group.

- **Description**: Use this box to describe the group so people know what the purpose of the group is. This description appears in the address book and in the Details pane in the Classic EAC.

- **Hide this group from address lists**: Select this check box if you don't want users to see this group in the address book. To send email to this group, a sender has to type the group's alias or email address on the To: or Cc: lines.

- **Organizational unit**: This read-only box displays the organizational unit (OU) that contains the dynamic distribution group. You have to use Active Directory Users and Computers to move the group to a different OU.

#### Ownership

Use this section to assign a group owner. A dynamic distribution group can have only one owner. The group owner appears on the **Managed by** tab of the object in Active Directory Users and Computers.

You can add owners by clicking **Browse** and selecting the owner from the list. To remove the owner, click **Clear** and then click **Save**.![Remove icon](../../media/ITPro_EAC_RemoveIcon.gif).

#### Membership

Use this section to change the criteria used to determine membership of the group. You can delete or change existing membership rules and add new rules. For procedures that tell you how to do this, see [Use the Classic EAC to create a dynamic distribution group](#use-the-classic-eac-to-create-a-dynamic-distribution-group) in the procedures for configuring membership when you use the Classic EAC to create a new dynamic distribution group.

#### Delivery management

Use this section to manage who can send email to this group.

- **Only senders inside my organization**: Select this option to allow only senders in your organization to send messages to the group. This means that if someone outside your organization sends an email message to this group, it is rejected. This is the default setting.

- **Senders inside and outside of my organization**: Select this option to allow anyone to send messages to the group.

  You can further limit who can send messages to the group by allowing only specific senders to send messages to this group. Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) and then select one or more recipients. If you add senders to this list, they are the only ones who can send mail to the group. Mail sent by anyone not in the list will be rejected.

  To remove a person or a group from the list, select them in the list and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.gif).

  > [!IMPORTANT]
  > If you've configured the group to allow only senders inside your organization to send messages to the group, email sent from a mail contact is rejected, even if they're added to this list.

#### Message approval 

Use this section to set options for moderating the group. Moderators approve or reject messages sent to the group before they reach the group members.

- **Messages sent to this group have to be approved by a moderator**: This check box isn't selected by default. If you select this check box, incoming messages are reviewed by the group moderators before delivery. Group moderators can approve or reject incoming messages.

- **Group moderators**: To add group moderators, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif). To remove a moderator, select the moderator, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.gif). If you've selected "Messages sent to this group have to be approved by a moderator" and you don't select a moderator, messages to the group are sent to the group owners for approval.

- **Senders who don't require message approval**: To add people or groups that can bypass moderation for this group, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif). To remove a person or a group, select the item, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.gif).

- **Select moderation notifications**: Use this section to set how users are notified about message approval.

  - **Notify all senders when their messages aren't approved**: This is the default setting. Notify all senders, inside and outside your organization, when their message isn't approved.

  - **Notify senders in your organization only when their messages aren't approved**: When you select this option, only people or groups in your organization are notified when a message that they sent to the group isn't approved by a moderator.

  - **Don't notify anyone when a message isn't approved**: When you select this option, notifications aren't sent to senders whose messages aren't approved by the group moderators.

#### Email options 

Use this section to view or change the email addresses associated with the group. This includes the group's primary SMTP addresses and any associated proxy addresses. The primary SMTP address (also known as the reply address) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

- **Add**: Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) to add a new email address for this mailbox. Select one of following address types:

  - **SMTP**: This is the default address type. Click this button and then type the new SMTP address in the **\* Email address** box.

    > [!NOTE]
    > To make the new address the primary SMTP address for the group, select the **Make this the reply address** check box.

  - **Custom address type**: Click this button and type one of the supported non-SMTP email address types in the **\* Email address** box.

    > [!NOTE]
    > With the exception of X.400 addresses, Exchange doesn't validate custom addresses for proper formatting. You must make sure that the custom address you specify complies with the format requirements for that address type.

- **Edit**: To change an email address associated with the group, select it from the list, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

    > [!NOTE]
    > To make an existing address the primary SMTP address for the group, select the **Make this the reply address** check box.

- **Remove**: To delete an email address associated with the group, select it from the list, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.gif).

- **Automatically update email addresses based on the email address policy applied to this recipient**: Select this check box to have the recipient's email addresses automatically updated based on changes made to email address policies in your organization. This box is selected by default.

#### MailTip

Use this section to add a MailTip to alert users of potential issues before they send a message to this group. A MailTip is text that's displayed in the InfoBar when this group is added to the To, Cc, or Bcc lines of a new email message. For example, you could add a MailTip to large groups to warn potential senders that their message will be sent to lots of people.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

#### Group delegation

Use this section to assign permissions to a user (called a delegate) to allow them to send messages as the group or send messages on behalf of the group. You can assign the following permissions:

- **Send As**: This permission allows the delegate to send messages as the group. After this permission is assigned, the delegate has the option to add the group to the **From** line to indicate that the message was sent by the group.

- **Send on Behalf Of**: This permission also allows a delegate to send messages on behalf of the group. After this permission is assigned, the delegate has the option to add the group on the **From** line. The message will appear to be sent by the group and will say that it was sent by the delegate on behalf of the group.

To assign permissions to delegates in Classic EAC, click **Add** under the appropriate permission to display the **Select Recipient** page, which displays a list of all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search**.

#### Use Exchange Online PowerShell to change dynamic distribution group properties

Use the **Get-DynamicDistributionGroup** and **Set-DynamicDistributionGroup** cmdlets to view and change properties for dynamic distribution groups. Advantages of using Exchange Online PowerShell are the ability to change the properties that aren't available in the EAC and change properties for multiple groups. For information about what parameters correspond to distribution group properties, see the following topics:

- [Get-DynamicDistributionGroup](https://docs.microsoft.com/powershell/module/exchange/get-dynamicdistributiongroup)

- [Set-DynamicDistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-dynamicdistributiongroup)

Here are some examples of using Exchange Online PowerShell to change dynamic distribution group properties.

This example changes the following parameters for all dynamic distribution groups in the organization:

- Hide all dynamic distribution groups from the address book

- Set the maximum message size that can be sent to the group to 5 MB

- Enable moderation

- Assign the administrator as the group moderator

```PowerShell
Get-DynamicDistributionGroup -ResultSize unlimited | Set-DynamicDistributionGroup -HiddenFromAddressListsEnabled $true -MaxReceiveSize 5MB -ModerationEnabled $true -ModeratedBy administrator
```

This example adds the proxy SMTP email address, Seattle.Employees@contoso.com, to the All Employees group.

```PowerShell
Set-DynamicDistributionGroup -Identity "All Employees" -EmailAddresses SMTP:All.Employees@contoso.com, smtp:Seattle.Employees@contoso.com
```

#### How do you know this worked?

To verify that you've successfully changed properties for a dynamic distribution group, do the following:

- In the new EAC, select the group to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the details pane for the selected group.

- In the Classic EAC, select the group and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif) to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the details pane for the selected group.

- In Exchange Online PowerShell, use the **Get-DynamicDistributionGroup** cmdlet to verify the changes. One advantage of using Exchange Online PowerShell is that you can view multiple properties for multiple groups. In the first example, you would run the following command to verify the new values.

  ```PowerShell
  Get-DynamicDistributionGroup -ResultSize unlimited | Format-List Name,HiddenFromAddressListsEnabled,MaxReceiveSize,ModerationEnabled,ModeratedBy
  ```

    For the example above where the message limits were changed, run this command.

  ```PowerShell
  Get-Mailbox -OrganizationalUnit "Marketing" | Format-List Name,IssueWarningQuota,ProhibitSendQuota,ProhibitSendReceiveQuota,UseDatabaseQuotaDefaults
  ```
