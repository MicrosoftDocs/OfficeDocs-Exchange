---
localization_priority: Normal
description: 'Summary: Learn how to create mail users and how to change mail user properties.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c72f46dc-8db3-486d-a998-0f01ffc1a843
ms.reviewer:
title: Manage mail users
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Manage mail users

Mail users are similar to mail contacts. Both have external email addresses and both contain information about people outside your Exchange organization that can be displayed in the shared address book and other address lists. However, unlike a mail contact, a mail user has logon credentials in your Exchange organization and can access resources. For more information, see [Recipients](recipients.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- To open the EAC, see [Exchange admin center in Exchange Server](../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Create a mail user

### Use the EAC to create a mail user

1. In the EAC, navigate to **Recipients** \> **Contacts** \> **New** \> **Mail user**.

2. On the **New mail user** page, in the **\* Alias** box, type the alias for the mail user. The alias can't exceed 64 characters and must be unique in the forest. This is required.

3. Do one of the following to specify the email address type for the mail user:

   - To specify an SMTP email address for the mail user's external email address, click **SMTP**.

    > [!NOTE]
    > Exchange validates SMTP addresses for correct formatting. If your entry is inconsistent with the SMTP format, an error message will be displayed when you click **Save** to create the mail user.

   - To specify a custom address type, click the option button and then type the custom address type. For example, you can specify an X.500, GroupWise, or Lotus Notes address.

4. In the **\* External email address** box, type the mail user's external email address. Email sent to this mail user is forwarded to this email address. This is required.

5. Select one of the following options:

   - **Existing user**: Select to mail-enable an existing user.

     Click **Browse** to open the **Select User - Entire Forest** dialog box. This dialog box displays a list of user accounts in the organization that aren't mail-enabled or don't have mailboxes. Select the user account you want to mail-enable, and then click **OK**. If you select this option, you don't have to provide user account information because this information already exists in Active Directory.

   - **New user**: Select to create a new user account in Active Directory and mail-enable the user. If you select this option, you'll have to provide the required user account information.

6. If you selected **New User** in Step 5, complete the following information on the **New mail user** page. Otherwise skip to Step 7.

   - **First name**: Type the first name of the mail user.

   - **Initials**: Type the initials of the mail user.

   - **Last name**: Type the last name of the mail user.

   - **\* Display name**: Use this box to type a display name for the user. This is the name that's listed in the contacts list in the EAC and in your organization's address book. By default, this box is populated with the names you enter in the **First name**, **Initials**, and **Last name** boxes. If you didn't use those boxes, you must still type a name in this box because it's required. The name can't exceed 64 characters.

   - **\* Name**: Use this box to type a name for the mail user. This is the name that's listed in the directory service. This box is also populated with the names you enter in the **First name**, **Initials**, and **Last name** boxes. If you didn't use those boxes, you must still type a name because this is required. This name also can't exceed 64 characters.

   - **Organizational unit**: You can select an organizational unit (OU) other than the default (which is the recipient scope). If the recipient scope is set to the forest, the default value is set to the Users container in the domain that contains the computer on which the EAC is running. If the recipient scope is set to a specific domain, the Users container in that domain is selected by default. If the recipient scope is set to a specific OU, that OU is selected by default.

     To select a different OU, click **Browse**. The dialog box displays all OUs in the forest that are within the specified scope. Select the OU you want, and then click **OK**.

   - **\* User logon name**: Type the name that the mail user will use to log on to the domain. The user logon name consists of a username on the left side of the at (@) symbol and a suffix on the right side. Typically, the suffix is the domain name the user account resides in.

   - **\* New Password**: Type the password that the mail user must use to log on to the domain. Make sure that the password you supply complies with the password length, complexity, and history requirements of the domain you're creating the user account in.

   - **\* Confirm password**: Use this box to confirm the password that you typed in the **Password** box.

   - **Require password change on next logon**: Select this check box if you want mail users to reset the password when they first log on to the domain.

     If you select this check box, at first logon, the new mail user will be prompted with a dialog box in which to change the password. The mail user won't be allowed to perform any tasks until the password is changed successfully.

7. When you've finished, click **Save** to create the mail user.

### Use the Exchange Management Shell to create a mail user

This example creates a mail-enabled user account for Jeffrey Zeng in Exchange Server 2016 with the following details:

- The name and display name is Jeffrey Zeng.

- The alias is jeffreyz.

- The external email address is jzeng@tailspintoys.com.

- The first name is Jeffrey and the last name is Zeng.

- The logon name is jeffreyz@contoso.com.

- The password is Pa$$word1.

- The mail user will be created in the default OU. To specify a different OU, you can use the _OrganizationalUnit_ parameter.

```PowerShell
New-MailUser -Name "Jeffrey Zeng" -Alias jeffreyz -ExternalEmailAddress jzeng@tailspintoys.com -FirstName Jeffrey -LastName Zeng -UserPrincipalName jeffreyz@contoso.com -Password (ConvertTo-SecureString -String 'Pa$$word1' -AsPlainText -Force)
```

For detailed syntax and parameter information, see [New-MailUser](https://docs.microsoft.com/powershell/module/exchange/new-mailuser).

### How do you know this worked?

To verify that you've successfully created a mail user, do one of the following:

- In the EAC, navigate to **Recipients** \> **Contacts**. The new mail user is displayed in the list of contacts. Under **Contact Type**, the type is **Mail user**.

- In the Exchange Management Shell, run the following command to display information about the new mail user.

  ```PowerShell
  Get-MailUser <Name> | Format-List Name,RecipientTypeDetails,ExternalEmailAddress
  ```

## Change mail user properties

After you create a mail user, you can make changes and set additional properties by using the EAC or the Exchange Management Shell.

You can also change properties for multiple user mailboxes at the same time. For more information, see [Use the EAC to bulk edit mail users](#use-the-eac-to-bulk-edit-mail-users).

The estimated time to complete this task will vary based on the number of properties you want to view or change.

### Use the EAC to change user mailbox properties

1. In the EAC, navigate to **Recipients** \> **Contacts**.

2. In the list of contacts, click the mail user that you want to change the properties for, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

3. On the mail user properties page, click one of the following sections to view or change properties.

   - [General](#general)

   - [Contact Information](#contact-information)

   - [Organization](#organization)

   - [Email Addresses](#email-addresses)

   - [Mail Flow Settings](#mail-flow-settings)

   - [Member Of](#member-of)

   - [MailTip](#mailtip)

#### General

Use the **General** section to view or change basic information about the mail user.

- **First name**, **Initials**, **Last name**

- **\* Name**: This is the name that's listed in Active Directory. If you change this name, it can't exceed 64 characters.

- **\* Display name**: This name appears in your organization's address book, on the To: and From: lines in email, and in the list of contacts in the EAC. This name can't contain empty spaces before or after the display name.

- **\* User logon name**: This is the name that the user uses to log on to the domain.

- **Hide from address lists**: Select this check box to prevent the mail user from appearing in the address book and other address lists that are defined in your Exchange organization. After you select this check box, users can still send messages to the recipient by using the email address.

- **Require password change on next logon**: Select this check box if you want the user to reset their password the next time they log on to the domain.

Click **More options** to view or change these additional properties:

- **Organizational unit**: This read-only box displays the organizational unit (OU) that contains the mail user account. You have to use Active Directory Users and Computers to move the account to a different OU.

- **Custom attributes**: This section displays the custom attributes defined for the mail user. To specify custom attribute values, click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png). You can specify up to 15 custom attributes for the recipient.

#### Contact Information

Use the **Contact Information** section to view or change the user's contact information. The information on this page is displayed in the address book. Click **More options** to display additional boxes.

> [!TIP]
> You can use the **State/Province** box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

#### Organization

Use the **Organization** section to record detailed information about the user's role in the organization. This information is displayed in the address book. Also, you can create a virtual organization chart that's accessible from email clients such as Outlook.

- **Title**: Use this box to view or change the recipient's title.

- **Department**: Use this box to view or change the department in which the user works. You can use this box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

- **Company**: Use this box to view or change the company for which the user works. You can use this box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

- **Manager**: To add a manager, click **Browse**. In **Select Manager**, select a person, and then click **OK**.

- **Direct reports**: You can't modify this box. A *direct report* is a user who reports to a specific manager. If you've specified a manager for the user, that user appears as a direct report in the details of the manager's mailbox. For example, Kari manages Chris and Kate, so Kari is specified in the **Manager** box for Chris and Kate, and Chris and Kate appear in the **Direct reports** box in the properties of Kari's account.

#### Email Addresses

Use the **Email Addresses** section to view or change the email addresses associated with the mail user. This includes the mail user's primary SMTP address, their external email address, and any associated proxy addresses. The primary SMTP address (also known as the *default reply address*) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column. By default, after the mail user is created, the primary SMTP address and the external email address are the same.

- **Add**: Click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) to add a new email address for this mailbox. Select one of following address types:

  - **SMTP**: This is the default address type. Click this button and then type the new SMTP address in the **\* Email address** box.

  - **Custom address type**: Click this button and type one of the supported non-SMTP email address types in the **\* Email address** box.

    **Note**: With the exception of X.400 addresses, Exchange doesn't validate custom addresses for correct formatting. You must make sure that the custom address you specify complies with the format requirements for that address type.

- **Set the external email address**: Use this box to change the mail user's external address. Email sent to this mail user is forwarded to this email address.

- **Automatically update email addresses based on the email address policy applied to this recipient**: Select this check box to have the recipient's email addresses automatically updated based on changes made to email address policies in your organization. This box is selected by default.

#### Mail Flow Settings

Use the **Mail Flow Settings** section to view or change the following settings:

- **Message Size Restrictions**: These settings control the size of messages that the mail user can send and receive. Click **View details** to view and change maximum size for sent and received messages.

  - **Sent messages**: To specify a maximum size for messages sent by this user, select the **Maximum message size (KB)** check box and type a value in the box. The message size must be between 0 and 2,097,151 KB. If the user sends a message larger than the specified size, the message will be returned to the user with a descriptive error message.

  - **Received messages**: To specify a maximum size for messages received by this user, select the **Maximum message size (KB)** check box and type a value in the box. The message size must be between 0 and 2,097,151 KB. If the user receives a message larger than the specified size, the message will be returned to the sender with a descriptive error message.

- **Message Delivery Restrictions**: These settings control who can send email messages to this mail user. Click **View details** to view and change these restrictions.

  - **Accept messages from**: Use this section to specify who can send messages to this user.

  - **All senders**: Select this option to specify that the user can accept messages from all senders. This includes both senders in your Exchange organization and external senders. This option is selected by default. This option includes external users only if you clear the **Require that all senders are authenticated** check box. If you select this check box, messages from external users will be rejected.

  - **Only senders in the following list**: Select this option to specify that the user can accept messages only from a specified set of senders in your Exchange organization. Click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) to display the **Select Recipients** page, which displays a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](../media/ITPro_EAC_.png).

  - **Require that all senders are authenticated**: Select this option to prevent anonymous users from sending messages to the user.

  - **Reject messages from**: Use this section to block people from sending messages to this user.

  - **No senders**: Select this option to specify that the mailbox won't reject messages from any senders in the Exchange organization. This option is selected by default.

  - **Senders in the following list**: Select this option to specify that the mailbox will reject messages from a specified set of senders in your Exchange organization. Click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) to display the **Select Recipients** page, which displays a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](../media/ITPro_EAC_.png).

#### Member Of

Use the **Member Of** section to view a list of the distribution groups or security groups to which this user belongs. You can't change membership information on this page. Note that the user may match the criteria for one or more dynamic distribution groups in your organization. However, dynamic distribution groups aren't displayed on this page because their membership is calculated each time they're used.

#### MailTip

Use the **MailTip** section to add a MailTip to alert users of potential issues before they send a message to this recipient. A MailTip is text that's displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc lines of a new email message.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

### Use the Exchange Management Shell to change mail user properties

Properties for a mail user are stored in both Active Directory and Exchange. In general, use the **Get-User** and **Set-User** cmdlets to view and change organization and contact information properties. Use the **Get-MailUser** and **Set-MailUser** cmdlets to view or change mail-related properties, such email addresses, the MailTip, custom attributes, and whether the mail user is hidden from address lists.

Use the **Get-MailUser** and **Set-MailUser** cmdlets to view and change properties for mail users. For information, see the following topics:

- [Get-User](https://docs.microsoft.com/powershell/module/exchange/get-user)

- [Set-User](https://docs.microsoft.com/powershell/module/exchange/set-user)

- [Get-MailUser](https://docs.microsoft.com/powershell/module/exchange/get-mailuser)

- [Set-MailUser](https://docs.microsoft.com/powershell/module/exchange/set-mailuser)

Here are some examples of using the Exchange Management Shell to change mail user properties.

This example sets the external email address for Pilar Pinilla.

```PowerShell
Set-MailUser "Pilar Pinilla" -ExternalEmailAddress pilarp@tailspintoys.com
```

This example hides all mail users from the organization's address book.

```PowerShell
Get-MailUser | Set-MailUser -HiddenFromAddressListsEnabled $true
```

This example sets the Company property for all mail users to Contoso.

```PowerShell
Get-User -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'mailuser'" | Set-User -Company Contoso
```

This example sets the CustomAttribute1 property to a value of ContosoEmployee for all mail users that have a value of Contoso in the Company property.

```PowerShell
Get-User -ResultSize unlimited -Filter "(RecipientTypeDetails -eq 'mailuser') -and (Company -eq 'Contoso')" | Set-MailUser -CustomAttribute1 ContosoEmployee
```

For detailed syntax and parameter information, see [Set-MailUser](https://docs.microsoft.com/powershell/module/exchange/set-mailuser).

### How do you know this worked?

To verify that you've successfully changed properties for mail users, do the following:

- In the EAC, select the mail user and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png) to view the property that you changed.

- In the Exchange Management Shell, use the **Get-User** and **Get-MailUser** cmdlets to verify the changes. One advantage of using the Exchange Management Shell is that you can view multiple properties for multiple mail contacts.

  ```PowerShell
  Get-MailUser | Format-List Name,CustomAttribute1
  ```

    In the example above where the Company property was set to Contoso for all mail contacts, run the following command to verify the changes:

  ```PowerShell
  Get-User -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'mailuser'" | Format-List Name,Company
  ```

    In the example above where all mail users had the CustomAttribute1 property set to ContosoEmployee, run the following command to verify the changes.

  ```PowerShell
  Get-MailUser | Format-List Name,CustomAttribute1
  ```

## Bulk edit mail users

You can also use the EAC to change selected properties for multiple mail users. When you select two or more mail users from the contacts list in the EAC, the properties that can be bulk edited are displayed in the Details pane. When you change one of these properties, the change is applied to all selected recipients.

When you bulk edit mail users, you can change the following property areas:

- **Contact Information**: Change shared properties such as street, postal code, and city name.

- **Organization**: Change shared properties such as department name, company name, and the manager that the selected mail contacts or mail users report to.

### Use the EAC to bulk edit mail users

1. In the EAC, navigate to **Recipients** \> **Contacts**.

2. In the list of contacts, select two or more mail users. You can't bulk edit a combination of mail contacts and mail users.

   **Note**: You can select multiple adjacent mail users by holding down the Shift key and clicking the first mail user, and then clicking the last mail user you want to edit. You can also select multiple mail users by holding down the Ctrl key and clicking each one that you want to edit.

3. In the Details pane, under **Bulk Edit**, click **Update** under **Contact Information** or **Organization**.

4. Make the changes on the properties page and then save your changes.

### How do you know this worked?

To verify that you've successfully bulk edited mail users, do one of the following:

- In the EAC, select each of the mail users that you bulk edited and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png) to view the properties that you changed.

- In the Exchange Management Shell, use the **Get-User** cmdlet to verify the changes. For example, say you used the bulk edit feature in the EAC to change the manager and the office for all mail users from a vendor company named A. Datum Corporation. To verify these changes, you could run the following command in the Exchange Management Shell:

  ```PowerShell
  Get-User -ResultSize unlimited -Filter "(RecipientTypeDetails -eq 'mailuser') -and (Company -eq 'Adatum')" | Format-List Name,Office,Manager
  ```
