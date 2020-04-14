---
localization_priority: Normal
description: Mail users are similar to mail contacts. Both have external email addresses and both contain information about people outside your Exchange or Exchange Online organization that can be displayed in the shared address book and other address lists. However, unlike a mail contact, a mail user has logon credentials in your Exchange or Office 365 organization and can access resources. For more information, see Recipients.
ms.topic: article
author: mattpennathe3rd
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Recipients.NewMailUserWizardForm.NewMailUserIntroductionWizardPage
ms.author: v-mapenn
ms.assetid: bb8b8804-f730-4ad7-9173-896a4965b90f
ms.reviewer: 
title: Manage mail users
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage mail users

Mail users are similar to mail contacts. Both have external email addresses and both contain information about people outside your Exchange or Exchange Online organization that can be displayed in the shared address book and other address lists. However, unlike a mail contact, a mail user has logon credentials in your Exchange or Office 365 organization and can access resources. For more information, see [Recipients](https://technet.microsoft.com/library/40300ed4-85a5-463d-bb3a-cf787bd44e9d.aspx).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Mailbox Permissions](https://technet.microsoft.com/library/5b690bcb-c6df-4511-90e1-08ca91f43b37.aspx) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Create a mail user

### Use the EAC to create a mail user

1. In the EAC, navigate to **Recipients** \> **Contacts** \> **New** \> **Mail user**.

2. On the **New mail user** page, complete the following boxes.

   - **First name**: Use this box to type the first name of the mail user.

   - **Initials**: Use this box to type the initials of the mail user.

   - **Last name**: Use this box to type the last name of the mail user.
   
   - **\* Display name**: Use this box to type a display name for the user. This is the name that's listed in the contacts list in the EAC and in your organization's address book. By default, this box is populated with the names you enter in the **First name**, **Initials**, and **Last name** boxes. If you didn't use those boxes, you must still type a name in this box because it's required. The name can't exceed 64 characters.
   
   - **\* Alias**: Use this box to type the alias for the mail user. The alias can't exceed 64 characters and must be unique in the forest. This box is required.

   - **External email address**: Use this box to type the mail user's external email address. Email sent to this mail user is forwarded to this email address.

   - **\* User ID**: Use this box to type the name that the mail user will use to log on to the domain. The user logon name consists of a username on the left side of the at (@) symbol and a suffix on the right side. Typically, the suffix is the domain name the user account resides in. This box is required.

   - **\* New Password**: Use this box to type the password that the mail user must use to log on to the domain.

   > [!NOTE]
   > Make sure that the password you supply complies with the password length, complexity, and history requirements of Azure Active Directory. For more information, see [Password policies that only apply to cloud user accounts](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy#password-policies-that-only-apply-to-cloud-user-accounts).

   - **\* Confirm password**: Use this box to confirm the password that you typed in the **Password** box.

3. When you've finished, click **Save** to create the mail user.

### Use Exchange Online PowerShell to create a mail user

This example creates a mail-enabled user account for Jeffrey Zeng with the following details:

- The name and display name is Jeffrey Zeng (if you don't use the _DisplayName_ parameter, the value of the _Name_ parameter is used for the display name).

- The alias is jeffreyz.

- The external email address is jzeng@tailspintoys.com.

- The first name is Jeffrey and the last name is Zeng.

- The logon name is jeffreyz@contoso.com.

- The password is Pa$$word1.

```PowerShell
New-MailUser -Name "Jeffrey Zeng" -Alias jeffreyz -ExternalEmailAddress jzeng@tailspintoys.com -FirstName Jeffrey -LastName Zeng -UserPrincipalName jeffreyz@contoso.com -Password (ConvertTo-SecureString -String 'Pa$$word1' -AsPlainText -Force)
```

This example creates a mail-enabled user account for Rene Valdes in Exchange Online.

```PowerShell
New-MailUser -Name "Rene Valdes" -Alias renev -ExternalEmailAddress renevaldes@fineartschool.edu -FirstName Rene -LastName Valdes -MicrosoftOnlineServicesID renev@contoso.com -Password (ConvertTo-SecureString -String 'P@ssw0rd' -AsPlainText -Force)
```

### How do you know this worked?

To verify that you've successfully created a mail user, do one of the following:

- In the EAC, navigate to **Recipients** \> **Contacts**. The new mail user is displayed in the list of contacts. Under **Contact Type**, the type is **Mail user**.

- In Exchange Online PowerShell, run the following command to display information about the new mail user.

  ```PowerShell
  Get-MailUser <Name> | Format-List Name,RecipientTypeDetails,ExternalEmailAddress
  ```

## Change mail user properties

After you create a mail user, you can make changes and set additional properties by using the EAC or Exchange Online PowerShell.

You can also change properties for multiple user mailboxes at the same time. For more information, see [Use the EAC to bulk edit mail users](#use-the-eac-to-bulk-edit-mail-users).

The estimated time to complete this task will vary based on the number of properties you want to view or change.

### Use the EAC to change user mailbox properties

1. In the EAC, navigate to **Recipients** \> **Contacts**.

2. In the list of contacts, click the mail user that you want to change the properties for, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif).

3. On the mail user properties page, click one of the following sections to view or change properties.

#### General
<a name="General"> </a>

Use the **General** section to view or change basic information about the mail user.

- **First name**, **Initials**, **Last name**

- **\* Name**: This is the name that's listed in Active Directory. If you change this name, it can't exceed 64 characters.

- **\* Display name**: This name appears in your organization's address book, on the To: and From: lines in email, and in the list of contacts in the EAC. This name can't contain empty spaces before or after the display name.

- **\* User logon name**: This is the name that the user uses to log on to the domain. In Exchange Online, this is the User ID that the user uses to sign in to Office 365.

- **Hide from address lists**: Select this check box to prevent the mail user from appearing in the address book and other address lists that are defined in your Exchange organization. After you select this check box, users can still send messages to the recipient by using the email address.

Click **More options** to view or change these additional properties:

- **Custom attributes**: This section displays the custom attributes defined for the mail user. To specify custom attribute values, click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif). You can specify up to 15 custom attributes for the recipient.

#### Contact Information
<a name="ContactInformation"> </a>

Use the **Contact Information** section to view or change the user's contact information. The information on this page is displayed in the address book. Click **More options** to display additional boxes.

> [!TIP]
> You can use the **State/Province** box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

#### Organization
<a name="Organization"> </a>

Use the **Organization** section to record detailed information about the user's role in the organization. This information is displayed in the address book. Also, you can create a virtual organization chart that's accessible from email clients such as Outlook.

- **Title**: Use this box to view or change the recipient's title.

- **Department**: Use this box to view or change the department in which the user works. You can use this box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

- **Company**: Use this box to view or change the company for which the user works. You can use this box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

- **Manager**: To add a manager, click **Browse**. In **Select Manager**, select a person, and then click **OK**.

- **Direct reports**: You can't modify this box. A direct report is a user who reports to a specific manager. If you've specified a manager for the user, that user appears as a direct report in the details of the manager's mailbox. For example, Kari manages Chris and Kate, so Kari is specified in the **Manager** box for Chris and Kate, and Chris and Kate appear in the **Direct reports** box in the properties of Kari's account.

#### Email Addresses
<a name="EmailAddress"> </a>

Use the **Email Addresses** section to view or change the email addresses associated with the mail user. This includes the mail user's primary SMTP address, their external email address, and any associated proxy addresses. The primary SMTP address (also known as the default reply address) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column. By default, after the mail user is created, the primary SMTP address and the external email address are the same.

- **Add**: Click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) to add a new email address for this mailbox. Select one of following address types:

  - **SMTP**: This is the default address type. Click this button and then type the new SMTP address in the **\* Email address** box.

  - **Custom address type**: Click this button and type one of the supported non-SMTP email address types in the **\* Email address** box.

   > [!NOTE]
   > With the exception of X.400 addresses, Exchange doesn't validate custom addresses for correct formatting. You must make sure that the custom address you specify complies with the format requirements for that address type.

- **Set the external email address**: Use this box to change the mail user's external address. Email sent to this mail user is forwarded to this email address.

#### Mail Flow Settings
<a name="Mailflow"> </a>

Use the **Mail Flow Settings** section to view or change the following settings:

- **Message Delivery Restrictions**: These settings control who can send email messages to this mail user. Click **View details** to view and change these restrictions.

   - **Accept messages from**: Use this section to specify who can send messages to this user.

   - **All senders**: Select this option to specify that the user can accept messages from all senders. This includes both senders in your Exchange organization and external senders. This option is selected by default. This option includes external users only if you clear the **Require that all senders are authenticated** check box. If you select this check box, messages from external users will be rejected.

   - **Only senders in the following list**: Select this option to specify that the user can accept messages only from a specified set of senders in your Exchange organization. Click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) to display the **Select Recipients** page, which displays a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](../media/ITPro_EAC_.gif).

   - **Require that all senders are authenticated**: Select this option to prevent anonymous users from sending messages to the user.

   - **Reject messages from**: Use this section to block people from sending messages to this user.

   - **No senders**: Select this option to specify that the mailbox won't reject messages from any senders in the Exchange organization. This option is selected by default.

   - **Senders in the following list**: Select this option to specify that the mailbox will reject messages from a specified set of senders in your Exchange organization. Click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) to display the **Select Recipients** page, which displays a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](../media/ITPro_EAC_.gif).

#### Member Of
<a name="MemberOf"> </a>

Use the **Member Of** section to view a list of the distribution groups or security groups to which this user belongs. You can't change membership information on this page. Note that the user may match the criteria for one or more dynamic distribution groups in your organization. However, dynamic distribution groups aren't displayed on this page because their membership is calculated each time they're used.

#### MailTip
<a name="MailTip"> </a>

Use the **MailTip** section to add a MailTip to alert users of potential issues before they send a message to this recipient. A MailTip is text that's displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc lines of a new email message.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

### Use Exchange Online PowerShell to change mail user properties

Properties for a mail user are stored in both Active Directory and Exchange. In general, use the **Get-User** and **Set-User** cmdlets to view and change organization and contact information properties. Use the **Get-MailUser** and **Set-MailUser** cmdlets to view or change mail-related properties, such email addresses, the MailTip, custom attributes, and whether the mail user is hidden from address lists.

Use the **Get-MailUser** and **Set-MailUser** cmdlets to view and change properties for mail users. For information, see the following topics:

- [Get-User](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/get-user)

- [Set-User](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/set-user)

- [Get-MailUser](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/get-mailuser)

- [Set-MailUser](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/set-mailuser)

Here are some examples of using Exchange Online PowerShell to change mail user properties.

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

### How do you know this worked?

To verify that you've successfully changed properties for mail users, do the following:

- In the EAC, select the mail user and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif) to view the property that you changed.

- In Exchange Online PowerShell, use the **Get-User** and **Get-MailUser** cmdlets to verify the changes. One advantage of using Exchange Online PowerShell is that you can view multiple properties for multiple mail contacts.

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
<a name="bulkedit"> </a>

1. In the EAC, navigate to **Recipients** \> **Contacts**.

2. In the list of contacts, select two or more mail users. You can't bulk edit a combination of mail contacts and mail users.

   > [!TIP]
   > You can select multiple adjacent mail users by holding down the Shift key and clicking the first mail user, and then clicking the last mail user you want to edit. You can also select multiple mail users by holding down the Ctrl key and clicking each one that you want to edit.

3. In the Details pane, under **Bulk Edit**, click **Update** under **Contact Information** or **Organization**.

4. Make the changes on the properties page and then save your changes.

### How do you know this worked?
<a name="bulkedit"> </a>

To verify that you've successfully bulk edited mail users, do one of the following:

- In the EAC, select each of the mail users that you bulk edited and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif) to view the properties that you changed.

- In Exchange Online PowerShell, use the **Get-User** cmdlet to verify the changes. For example, say you used the bulk edit feature in the EAC to change the manager and the office for all mail users from a vendor company named A. Datum Corporation. To verify these changes, you could run the following command in Exchange Online PowerShell:

   ```PowerShell
   Get-User -ResultSize unlimited -Filter "(RecipientTypeDetails -eq 'mailuser') -and (Company -eq 'Adatum')" | Format-List Name,Office,Manager
   ```

## Use directory synchronization to manage mail users in Exchange Online

This section provides information about managing email users by using directory synchronization in Exchange Online. Directory synchronization is available for hybrid customers with on-premises and cloud-hosted mailboxes, and for fully hosted Exchange Online customers whose Active Directory is on-premises.

**Notes**:

- If you use directory synchronization to manage your recipients, you can still add and manage users in the Microsoft 365 admin center, but they will not be synchronized with your on-premises Active Directory. This is because directory synchronization only syncs recipients from your on-premises Active Directory to the cloud.

- Using directory synchronization is recommended for use with the following features:

   - **Outlook safe sender and blocked sender lists**: When synchronized to the service, these lists will take precedence over spam filtering in the service. This lets users manage their own safe sender and blocked sender lists on a per-user or per-domain basis.

   - **Directory Based Edge Blocking (DBEB)**: For more information about DBEB, see [Use Directory Based Edge Blocking to reject messages sent to invalid recipients](../mail-flow-best-practices/use-directory-based-edge-blocking.md).

   - **End user spam quarantine**: In order to access the end user spam quarantine, end users must have a valid Office 365 user ID and password. Customers with on-premises mailboxes must be valid email users. >

   - **Mail flow rules (also known as transport rules)**: When you use directory synchronization, your existing Active Directory users and groups are automatically uploaded to the cloud, and you can then create mail flow rules that target specific users and/or groups without having to manually add them via the EAC or Exchange Online PowerShell. Note that [dynamic distribution groups](https://go.microsoft.com/fwlink/p/?LinkId=507569) can't be synchronized via directory synchronization.

### Before you begin

Get the necessary permissions and prepare for directory synchronization, as described in [Prepare for directory synchronization](https://go.microsoft.com/fwlink/p/?LinkId=308908).

### To synchronize user directories

1. Activate directory synchronization, as described in [Activate directory synchronization](https://go.microsoft.com/fwlink/p/?LinkId=308909).

2. Set up your directory synchronization computer, as described in [Set up your directory sync computer](https://go.microsoft.com/fwlink/p/?LinkId=308911).

3. Synchronize your directories, as described in [Use the Configuration Wizard to sync your directories](https://go.microsoft.com/fwlink/p/?LinkId=308912).

   > [!IMPORTANT]
   > When you finish the Azure Active Directory Sync Tool Configuration Wizard, the **MSOL_AD_SYNC** account is created in your Active Directory forest. This account is used to read and synchronize your on-premises Active Directory information. In order for directory synchronization to work correctly, make sure that TCP 443 on your local directory synchronization server is open.

4. Activate synced users, as described in [Activate synced users](https://go.microsoft.com/fwlink/p/?LinkId=308913).

5. Manage directory synchronization, as described in [Manage directory synchronization](https://go.microsoft.com/fwlink/p/?LinkId=308915).

6. Verify that Exchange Online is synchronizing correctly. In the EAC, go to **Recipients** \> **Contacts** and view that the list of users was correctly synchronized from your on-premises environment.
