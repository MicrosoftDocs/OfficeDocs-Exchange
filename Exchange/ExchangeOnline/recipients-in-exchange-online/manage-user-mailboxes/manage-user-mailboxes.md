---
localization_priority: Normal
description: After you create a user mailbox, you can make changes and set additional properties by using the EAC or Exchange Online PowerShell.
ms.topic: article
author: mattpennathe3rd
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Recipients.NewMailboxWizardForm.NewMailboxIntroductionWizardPage
ms.author: v-mapenn
ms.assetid: 957ca61c-1fa1-42ab-a0e6-8488e4782566
ms.reviewer: 
title: Manage user mailboxes
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage user mailboxes

After you create a user mailbox, you can make changes and set additional properties by using the EAC or Exchange Online PowerShell.

## What do you need to know before you begin?

- Estimated time to complete each user mailbox task: 2 to 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" section in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Change user mailbox properties

### Use the EAC to change user mailbox properties

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to change the properties for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, you can change any of the following properties.

   - **General**

   - **Mailbox Usage**

   - **Contact Information**

   - **Organization**

   - **Email Address**

   - **Mailbox Features**

   - **Member Of**

   - **MailTip**

   - **Mailbox Delegation**

#### General

Use the **General** section to view or change basic information about the user.

- **First name**, **Initials**, **Last name**

- **\* Name**: This is the name that's listed in Active Directory. If you change this name, it can't exceed 64 characters.

- **\* Display name**: This name appears in your organization's address book, on the To: and From: lines in email, and in the Mailbox list. This name can't contain empty spaces before or after the display name.

- **\* Alias**: This specifies the email alias for the user. The user's alias is the portion of the email address on the left side of the at (@) symbol. It must be unique in the forest.

- **\* User ID**: This is the name that the user uses to sign in to their mailbox and to log on to the domain. Typically the user logon name consists of the user's alias on the left side of the @ symbol, and the domain name in which the user account resides on the right side of the @ symbol.

- **Hide from address lists**: Select this check box to prevent the recipient from appearing in the address book and other address lists that are defined in your Exchange organization. After you select this check box, users can still send messages to the recipient by using the email address.

Click **More options** to view or change these additional properties:

- **Custom attributes**: This section displays the custom attributes defined for the user mailbox. To specify custom attribute values, click **Edit**. You can specify up to 15 custom attributes for the recipient.

#### Mailbox Usage

Use the **Mailbox Usage** section to view or change the mailbox storage quota and deleted item retention settings for the mailbox. These settings are configured by default when the mailbox is created. They use the values that are configured for the mailbox database and apply to all mailboxes in that database. You can customize these settings for each mailbox instead of using the mailbox database defaults.

- **Last logon**: This read-only box displays the last time that the user signed in to their mailbox.

- **Mailbox usage**: This area shows the total size of the mailbox and the percentage of the total mailbox quota that has been used.

> [!NOTE]
> To obtain the information that's displayed in the previous two boxes, the EAC queries the mailbox database that hosts the mailbox. If the EAC is unable to communicate with the Exchange store that contains the mailbox database, these boxes will be blank. A warning message is displayed if the user hasn't signed in to the mailbox for the first time.

#### Contact Information

Use the **Contact Information** section to view or change the user's contact information. The information on this page is displayed in the address book. Click **More options** to display additional boxes.

> [!TIP]
> You can use the **State/Province** box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

Mailbox users can use Outlook or Outlook on the web (formerly known as Outlook Web App) to view and change their own contact information. But they can't change the information in the **Notes** and **Web page** boxes.

#### Organization

Use the **Organization** section to record detailed information about the user's role in the organization. This information is displayed in the address book. Also, you can create a virtual organization chart that is accessible from email clients such as Outlook.

- **Title**: Use this box to view or change the recipient's title.

- **Department**: Use this box to view or change the department in which the user works. You can use this box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

- **Company**: Use this box to view or change the company for which the user works. You can use this box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

- **Manager**: To add a manager, click **Browse**. In **Select Manager**, select a person, and then click **OK**.

- **Direct reports**: You can't modify this box. A direct report is a user who reports to a specific manager. If you've specified a manager for the user, that user appears as a direct report in the details of the manager's mailbox. For example, Kari manages Chris and Kate, so Kari's mailbox is specified in the **Manager** box of Chris's mailbox and Kate's mailbox, and Chris and Kate appear in the **Direct reports** box in the properties of Kari's mailbox.

#### Email Address

Use the **Email Address** section to view or change the email addresses associated with the user mailbox. This includes the user's primary SMTP address and any associated proxy addresses. The primary SMTP address (also known as the default reply address) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

- **Add**: Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) to add a new email address for this mailbox. Select one of following address types:

  - **SMTP**: This is the default address type. Click this button and then type the new SMTP address in the **\* Email address** box.

  - **Custom address type**: Click this button and type one of the supported non-SMTP email address types in the **\* Email address** box.

    > [!NOTE]
    > With the exception of X.400 addresses, Exchange doesn't validate custom addresses for proper formatting. You must make sure that the custom address you specify complies with the format requirements for that address type.

  - **Make this the reply address**: In Exchange Online, you can select this check box to make the new email address the primary SMTP address for the mailbox. This check box isn't available in the EAC in Exchange Server.

#### Mailbox Features

Use the **Mailbox Features** section to view or change the following mailbox features and settings:

- **Sharing policy**: This box shows the sharing policy applied to the mailbox. A sharing policy controls how users in your organization can share calendar and contact information with users outside your Exchange organization. The Default Sharing Policy is assigned to mailboxes when they are created. To change the sharing policy that's assigned to the user, select a different one from the drop-down list.

- **Role assignment policy**: This box shows the role assignment policy assigned to the mailbox. The role assignment policy specifies the role-based access control (RBAC) roles that are assigned to the user and control what specific mailbox and distribution group configuration settings users can modify. To change the role assignment policy that's assigned to the user, select a different one from the drop-down list.

- **Retention policy**: This box shows the retention policy assigned to the mailbox. A retention policy is a group of retention tags that are applied to the user's mailbox. They allow you to control how long to keep items in users' mailboxes and define what action to take on items that have reached a certain age. A retention policy isn't assigned to mailboxes when they are created. To assign a retention policy to the user, select one from the drop-down list.

- **Address book policy**: This box shows the address book policy applied to the mailbox. An address book policy allows you to segment users into specific groups to provide customized views of the address book. To apply or change the address book policy applied to the mailbox, select one from the drop-down list.

- **Mobile Devices**: Use this section to view and change the settings for Exchange ActiveSync, which is enabled by default. Exchange ActiveSync enables access to an Exchange mailbox from a mobile device. Click **Disable Exchange ActiveSync** to disable this feature for the mailbox.

- **Outlook on the web**: This feature is enabled by default. Outlook on the web enables access to an Exchange mailbox from a web browser. Click **Disable** to disable Outlook on the web for the mailbox. Click **Edit details** to add or change an Outlook on the web mailbox policy for the mailbox.

- **IMAP**: This feature is enabled by default. Click **Disable** to disable IMAP for the mailbox.

- **POP3**: This feature is enabled by default. Click **Disable** to disable POP3 for the mailbox.

- **MAPI**: This feature is enabled by default. MAPI enables access to an Exchange mailbox from a MAPI client such as Outlook. Click **Disable** to disable MAPI for the mailbox.

- **Litigation hold**: This feature is disabled by default. Litigation hold preserves deleted mailbox items and records changes made to mailbox items. Deleted items and all instances of changed items are returned in a discovery search. Click **Enable** to put the mailbox on litigation hold. If the mailbox is on litigation hold, click **Disable** to remove the litigation hold. Mailboxes on litigation hold are inactive mailboxes and can't be deleted. To delete the mailbox, remove the litigation hold. If the mailbox is on litigation hold, click **Edit details** to view and change the following litigation hold settings:

  - **Hold date**: This read-only box indicates the date and time when the mailbox was put on litigation hold.

  - **Put on hold by**: This read-only box indicates the user who put the mailbox on litigation hold.

  - **Note**: Use this box to notify the user about the litigation hold, explain why the mailbox is on litigation hold, or provide additional guidance to the user, such as informing them that the litigation hold won't affect their day-to-day use of email.

  - **URL**: Use this box to provide a URL to a website that provides information or guidance about the litigation hold on the mailbox.

    > [!NOTE]
    > The text from these boxes appears in the user's mailbox only if they are using Outlook 2010 or later versions. It doesn't appear in Outlook on the web or other email clients. To view the text from the Note and URL boxes in Outlook, click the **File** tab, and on the **Info** page, under **Account Settings**, you'll see the litigation hold comment.

- **Archiving**: If an archive mailbox doesn't exist for the user, this feature is disabled. To enable an archive mailbox, click **Enable**. If the user has an archive mailbox, the size of the archive mailbox and usage statistics are displayed. Click **Edit details** to view and change the following archive mailbox settings:

  - **Status**: This read-only box indicates whether an archive mailbox exists.

  - **Database**: This read-only box shows the name of the mailbox database that hosts the archive mailbox. This box isn't available in Exchange Online.

  - **Name**: Type the name of the archive mailbox in this box. This name is displayed under the folder list in Outlook or Outlook on the web.

  - **Archive quota (GB)**: This box shows the total size of the archive mailbox.

  - **Issue warning at (GB)**: This box shows the maximum storage limit for the archive mailbox before a warning is issued to the user. If the archive mailbox size reaches or exceeds the value specified, Exchange sends a warning message to the user.

    > [!NOTE]
    > The archive quota and the issue warning quota for the archive mailbox can't be changed in Exchange Online.

- **Delivery Options**: Use to forward email messages sent to the user to another recipient and to set the maximum number of recipients that the user can send a message to. Click **View details** to view and change these settings.

  - **Forwarding address**: Select the **Enable forwarding** check box and then click **Browse** to display the **Select Mail User and Mailbox** page. Use this page to select a recipient to whom you want to forward all email messages that are sent to this mailbox.

  - **Deliver message to both forwarding address and mailbox**: Select this check box so that messages will be delivered to both the forwarding address and the user's mailbox.

  - **Recipient limit**: This setting controls the maximum number of recipients the user can send a message to. Select the **Maximum recipients** check box to limit the number of recipients allowed in the To:, Cc:, and Bcc: boxes of an email message and then specify the maximum number of recipients. In Exchange Online, the limit is 500 recipients.

- **Message Size Restrictions**: These settings control the size of messages that the user can send and receive. Click **View details** to view maximum size for sent and received messages.

    > [!NOTE]
    > These settings can't be changed in Exchange Online.

- **Message Delivery Restrictions**: These settings control who can send email messages to this user. Click **View details** to view and change these restrictions.

  - **Accept messages from**: Use this section to specify who can send messages to this user.

  - **All senders**: Select this option to specify that the user can accept messages from all senders. This includes both senders in your Exchange organization and external senders. This option is selected by default. This option includes external users only if you clear the **Require that all senders are authenticated** check box. If you select this check box, messages from external users will be rejected.

  - **Only senders in the following list**: Select this option to specify that the user can accept messages only from a specified set of senders in your Exchange organization. Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) to display the **Select Recipients** page, which displays a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](../../media/ITPro_EAC_.gif).

  - **Require that all senders are authenticated**: Select this option to prevent anonymous users from sending messages to the user.

  - **Reject messages from**: Use this section to block people from sending messages to this user.

  - **No senders**: Select this option to specify that the mailbox won't reject messages from any senders in the Exchange organization. This option is selected by default.

  - **Senders in the following list**: Select this option to specify that the mailbox will reject messages from a specified set of senders in your Exchange organization. Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) to display the **Select Recipients** page, which displays a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](../../media/ITPro_EAC_.gif).

#### Member Of

Use the **Member Of** section to view a list of the distribution groups or security groups to which this user belongs. You can't change membership information on this page. Note that the user may match the criteria for one or more dynamic distribution groups in your organization. However, dynamic distribution groups aren't displayed on this page because their membership is calculated each time they are used.

#### MailTip

Use the **MailTip** section to add a MailTip to alert users of potential issues if they send a message to this recipient. A MailTip is text that is displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc boxes of a new email message.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

#### Mailbox Delegation

Use the **Mailbox Delegation** section to assign permissions to other users (also called delegates) to allow them to sign in to the user's mailbox or send messages on behalf of the user. You can assign the following permissions:

- **Send As**: This permission allows users other than the mailbox owner to use the mailbox to send messages. After this permission is assigned to a delegate, any message that a delegate sends from this mailbox will appear as if it was sent by the mailbox owner. However, this permission doesn't allow a delegate to sign in to the user's mailbox.

- **Send on Behalf Of**: This permission also allows a delegate to use this mailbox to send messages. However, after this permission is assigned to a delegate, the **From:** address in any message sent by the delegate indicates that the message was sent by the delegate on behalf of the mailbox owner.

- **Full Access**: This permission allows a delegate to sign in to the user's mailbox and view the contents of the mailbox. However, after this permission is assigned to a delegate, the delegate can't send messages from the mailbox. To allow a delegate to send email from the user's mailbox, you still have to assign the delegate the Send As or the Send on Behalf Of permission.

To assign permissions to delegates, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) under the appropriate permission to display a page that displays a list of all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](../../media/ITPro_EAC_.gif).

### Use Exchange Online PowerShell to change user mailbox properties

Use the **Get-Mailbox** and **Set-Mailbox** cmdlets to view and change properties for user mailboxes. One advantage of using Exchange Online PowerShell is the ability to change the properties for multiple mailboxes. For information about what parameters correspond to mailbox properties, see the following topics:

- [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox)

Here are some examples of using Exchange Online PowerShell to change user mailbox properties.

This example shows how to forward Pat Coleman's email messages to Sunil Koduri's (sunilk@contoso.com) mailbox.

```PowerShell
Set-Mailbox -Identity patc -DeliverToMailboxAndForward $true -ForwardingAddress sunilk@contoso.com
```

This example uses the **Get-Mailbox** command to find all user mailboxes in the organization, and then uses the **Set-Mailbox** command to set the recipient limit to 500 recipients allowed in the To:, Cc:, and Bcc: boxes of an email message.

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Set-Mailbox -RecipientLimits 500
```

This example uses the **Get-Mailbox** command to find all the mailboxes in the Marketing organizational unit, and then uses the **Set-Mailbox** command to configure these mailboxes. The custom warning, prohibit send, and prohibit send and receive limits are set to 200 megabytes (MB), 250 MB, and 280 MB respectively, and the mailbox database's default limits are ignored. This command can be used to configure a specific set of mailboxes to have larger or smaller limits than other mailboxes in the organization.

```PowerShell
Get-Mailbox -OrganizationalUnit "Marketing" | Set-Mailbox -IssueWarningQuota 209715200 -ProhibitSendQuota 262144000 -ProhibitSendReceiveQuota 293601280 -UseDatabaseQuotaDefaults $false
```

This example uses the **Get-Mailbox** cmdlet to find all users in the Customer Service department, and then uses the **Set-Mailbox** cmdlet to change the maximum message size for sending messages to 2 MB.

```PowerShell
Get-Mailbox -Filter "Department -eq 'Customer Service'" | Set-Mailbox -MaxSendSize 2097152
```

This example sets the MailTip translation in French and Chinese.

```PowerShell
Set-Mailbox john@contoso.com -MailTipTranslations ("FR: C'est la langue française", "CHT: 這是漢語語言")
```

### How do you know this worked?

To verify that you've successfully changed properties for a user mailbox, do the following:

- In the EAC, select the mailbox and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif) to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the Details pane for the selected mailbox.

- In Exchange Online PowerShell, use the **Get-Mailbox** cmdlet to verify the changes. One advantage of using Exchange Online PowerShell is that you can view multiple properties for multiple mailboxes. In the example above where the recipient limit was changed, run the following command to verify the new value.

  ```PowerShell
  Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Format-List Name,RecipientLimits
  ```

    For the example above where the message limits were changed, run this command.

  ```PowerShell
  Get-Mailbox -OrganizationalUnit "Marketing" | Format-List Name,IssueWarningQuota,ProhibitSendQuota,ProhibitSendReceiveQuota,UseDatabaseQuotaDefaults
  ```

## Bulk edit user mailboxes

You can use the EAC to change the properties for multiple user mailboxes. When you select two or more user mailboxes from the mailbox list in the EAC, the properties that can be bulk edited are displayed in the Details pane. When you change one of these properties, the change is applied to all selected mailboxes.

Here's a list of the user mailbox properties and features that can be bulk edited. Note that not all properties in each area are available to be changed.

- **Contact Information**: Change shared properties such as street, postal code, and city name.

- **Organization**: Change shared properties such as department name, company name, and the manager that the selected users report to.

- **Custom attributes**: Change or add values for custom attributes 1 - 15.

- **Mailbox quota**: Change the mailbox quota values and the retention period for deleted items. This isn't available in Exchange Online.

- **Email connectivity**: Enable or disable Outlook on the web, POP3, IMAP, MAPI, and Exchange ActiveSync.

- **Archive**: Enable or disable the archive mailbox.

- **Retention policy, role assignment policy, and sharing policy**: Update the settings for each of these mailbox features.

- **Move mailboxes to another database**: Move the selected mailboxes to a different database.

- **Delegate permissions**: Assign permissions to users or groups that allow them to open or send messages from other mailboxes. You can assign Full, Send As and Send on Behalf permissions to users or groups. Check out [Manage permissions for recipients](../../recipients-in-exchange-online/manage-permissions-for-recipients.md) for more details.

> [!NOTE]
> The estimated time to complete this task is 2 minutes, but may take longer if you change multiple properties or features.

### Use the EAC to bulk edit user mailboxes

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of mailboxes, select two or more mailboxes.

    > [!TIP]
    > You can select multiple adjacent mailboxes by holding down the Shift key and clicking the first mailbox, and then clicking the last mailbox you want to edit. You can also select multiple non-adjacent mailboxes by holding down the Ctrl key and clicking each mailbox that you want to edit.

3. In the Details pane, under **Bulk Edit**, select the mailbox properties or feature that you want to edit.

4. Make the changes on the properties page and then save your changes.

### How do you know this worked?

To verify that you've successfully bulk edited user mailboxes, do one of the following:

- In the EAC, select each of the mailboxes that you bulk edited and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif) to view the property or feature that you changed.

- In Exchange Online PowerShell, use the **Get-Mailbox** cmdlet to verify the changes. One advantage of using Exchange Online PowerShell is that you can view multiple properties for multiple mailboxes. For example, say you used the bulk edit feature in the EAC to enable the archive mailbox and assign a retention policy to all users in your organization. To verify these changes, you could run the following command:

  ```PowerShell
  Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Format-List Name,ArchiveDatabase,RetentionPolicy
  ```

    For more information about the available parameters for the **Get-Mailbox** cmdlet, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox).
