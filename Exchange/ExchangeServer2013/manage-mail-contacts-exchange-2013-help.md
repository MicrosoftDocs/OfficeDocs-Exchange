---
title: 'Manage mail contacts: Exchange 2013 Help'
TOCTitle: Manage mail contacts
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.custom:
- 'Microsoft.Exchange.Management.SnapIn.Esm.Recipients.NewMailContactWizardForm.NewMailContactIntroductionWizardPage'
ms.assetid: 74c72aed-e9ff-4927-8eb7-c08a86e79ae0
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
---

# Manage mail contacts in Exchange 2013

_**Applies to:** Exchange Server 2013_

Mail contacts are mail-enabled directory service objects that contain information about people or organizations that exist outside your Exchange organization. Each mail contact has an external email address. For more information about mail contacts, see [Recipients](recipients-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Create a mail contact

### Use the EAC to create a mail contact

1. In the EAC, navigate to **Recipients** \> **Contacts**.

2. Click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif) \> **Mail contact**.

3. Complete the following boxes on the **New mail contact** page:

   - **First name**: Use this box to type the contact's first name.

   - **Initials**: Use this box to type the contact's initials.

   - **Last name** Use this box to type the contact's last name.

   - **\* Display name**: Use this box to type a display name for the contact. This is the name that's listed in the contacts list in the EAC and in your organization's address book. By default, this box is populated with the names you enter in the **First name**, **Initials**, and **Last name** boxes. If you didn't use those boxes, you must still type a name in this box because it's required. The name can't exceed 64 characters.

   - **\* Name**: Use this box to type a name for the contact. This is the name that's listed in the directory service. Like the display name, this box is populated by default with the names you enter in the **First name**, **Initials**, and **Last name** boxes. If you didn't use those boxes, you must still type a name in this box because it's required. The name can't exceed 64 characters.

   - **\* Alias**: Use this box to type an alias (64 characters or less) for the contact. This box is required.

   - **\* External email address**: Use this box to type the outside email account of the contact. This box is required. Email sent to this contact is forwarded to this email address.

   - **Organizational unit**: You can select an organizational unit (OU) other than the default, which is the recipient scope. If the recipient scope is set to the forest, the default value is set to the Users container in the domain that contains the computer on which the EAC is running. If the recipient scope is set to a specific domain, the Users container in that domain is selected by default. If the recipient scope is set to a specific OU, that OU is selected by default.

     To select a different OU, click **Browse**. The dialog box displays all OUs in the forest that are within the specified scope. Select the OU you want, and then click **OK**.

4. When you've finished, click **Save**.

### Use the Shell to create a mail contact

This example creates a mail contact for Debra Garcia.

```powershell
New-MailContact -Name "Debra Garcia" -ExternalEmailAddress dgarcia@tailspintoys.com -OrganizationalUnit Users
```

```powershell
Enable-MailContact -Identity "Karen Toh" -ExternalEmailAddress ktoh@tailspintoys.com
```

### How do you know this worked?

To verify that you've successfully created a mail contact, do one of the following:

- In the EAC, navigate to **Recipients** \> **Contacts**. The new mail contact is displayed in the contact list. Under **Contact Type**, the type is **Mail contact**.

- In the Shell, run the following command to display information about the new mail contact.

  ```powershell
  Get-MailContact <Name> | Format-List Name,RecipientTypeDetails,ExternalEmailAddress
  ```

## Change mail contact properties

### Use the EAC to change mail contact properties

1. In the EAC, navigate to **Recipients** \> **Contacts**.

2. In the list of mail contacts and mail users, click the mail contact that you want to change the properties for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mail contact properties page, click one of the following sections to view or change properties.

### General

Use the **General** section to view or change basic information about the mail contact.

- **First name**, **Initials**, **Last name**

- **\* Name**: This is the name that's listed in Active Directory. If you change this name, it can't exceed 64 characters.

- **\* Display name**: This name appears in your organization's address book, on the To and From lines in email, and in the Mailbox list. This name can't contain empty spaces before or after the display name.

- **\* Alias**: This is the mail contact's alias. If you change it, it must be unique in the organization and must be 64 characters or less.

- **\* External email address**: This is mail contact's primary SMTP address and their outside email account. Email sent to this contact is forwarded to this email address.

- Click **More options**: to display the OU that contains the mail contact account. You have to use Active Directory Users and Computers to move the contact to a different OU.

### Contact Information

Use the **Contact Information** section to view or change the recipient's contact information, such as mailing address and telephone numbers. This information is displayed in the address book.

### Organization

Use the **Organization** section to record detailed information about the mail contact's role in the organization. This information is displayed in the address book. Also, you can create a virtual organization chart that's accessible from email clients such as Outlook.

- **Title**: Use this box to view or change the contact's title.

- **Department**: Use this box to view or change the department in which the contact works. You can use this box to create recipient conditions for dynamic distribution groups and address lists.

- **Company**: Use this box to view or change the company for which the contact works. You can also use this box to create recipient conditions for dynamic distribution groups.

- **Manager**: To add a manager, click **Browse**. In **Select Manager**, select a person, and then click **OK**.

- **Direct reports**: You can't modify this box. A direct report is a recipient who reports to a specific manager. If you've specified a manager for the recipient, that recipient appears as a direct report in the details of the manager's mailbox. For example, Toby manages Ann and Spencer, who are mail contacts, so Toby is specified in the **Manager** box in the organization properties for Ann and Spencer, and Ann and Spencer appear in the **Direct reports** box in the properties of Toby's mailbox.

### Email Options

Use the **Email Options** section to add or remove proxy addresses for the mail contact or edit existing proxy addresses. The mail contact's primary SMTP address is also displayed in this section, but you can't change it. To change it, you have to change the contact's external email address in the **General** section.

### MailTip

Use the **MailTip** section to add a MailTip to alert users of potential issues before they send a message to this recipient. A MailTip is text that's displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc lines of a new email message.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

### Use the Shell to change mail contact properties

Properties for a mail contact are stored in both Active Directory and Exchange. In general, use the **Get-Contact** and **Set-Contact** cmdlets to view and change organization and contact information properties. Use the **Get-MailContact** and **Set-MailContact** cmdlets to view or change mail-related properties, such as email addresses, the MailTip, custom attributes, and whether the contact is hidden from address lists.

For more information, see the following topics:

- [Get-Contact](https://docs.microsoft.com/powershell/module/exchange/get-contact)

- [Set-Contact](https://docs.microsoft.com/powershell/module/exchange/set-contact)

- [Get-MailContact](https://docs.microsoft.com/powershell/module/exchange/get-mailcontact)

- [Set-MailContact](https://docs.microsoft.com/powershell/module/exchange/set-mailcontact)

Here are some examples of using the Shell to change mail contact properties.

This example configures the Title, Department, Company, and Manager properties for the mail contact Kai Axford.

```powershell
Set-Contact "Kai Axford" _-Title Consultant -Department "Public Relations" -Company Fabrikam -Manager "Karen Toh"
```

This example sets the CustomAttribute1 property to a value of PartTime for all mail contacts and hides them from the organization's address book.

```powershell
Get-MailContact | Set-MailContact -CustomAttribute1 PartTime -HiddenFromAddressListsEnabled $true
```

This example sets the CustomAttribute15 property to a value of TemporaryEmployee for all mail contacts in the Public Relations department.

```powershell
Get-Contact -Filter "Department -eq 'Public Relations'" | Set-MailContact -CustomAttribute15 TemporaryEmployee
```

### How do you know this worked?

To verify that you've successfully changed properties for a mail contact, do the following:

- In the EAC, select the mail contact, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif) to view the property that you changed.

- In the Shell, use the **Get-Contact** and **Get-MailContact** cmdlets to verify the changes. One advantage of using the Shell is that you can view multiple properties for multiple mail contacts. In the example above where all mail contacts had the CustomAttribute1 property set to PartTime and were hidden from the address book, run the following command to verify the changes.

  ```powershell
  Get-MailContact | Format-List Name,CustomAttribute1,HiddenFromAddressListsEnabled
  ```

    In the example above where the CustomAttribute15 was set for all mail contacts in the Public Relations department, run the following command to verify the changes.

  ```powershell
  Get-Contact -Filter "Department -eq 'Public Relations'" | Get-MailContact | Format-List Name,CustomAttribute15
  ```

## Bulk edit mail contacts

You can use the EAC to change selected properties for multiple mail contacts. When you select two or more mail contacts from the contacts list in the EAC, the properties that can be bulk edited are displayed in the Details pane. When you change one of these properties, the change is applied to all selected recipients.

When you bulk edit mail contacts, you can change the following property areas:

- **Contact Information** Change shared properties such as street, postal code, and city name.

- **Organization** Change shared properties such as department name, company name, and the manager that the selected mail contacts or mail users report to.

### Use the EAC to bulk edit mail contacts

1. In the EAC, navigate to **Recipients** \> **Contacts**.

2. In the list of contacts, select two or more mail contacts. You can't bulk edit a combination of mail contacts and mail users.

    > [!TIP]
    > You can select multiple adjacent mail contacts by holding down the Shift key and clicking the first mail contact, and then clicking the last mail contact you want to edit. You can also select multiple mail contacts by holding down the Ctrl key and clicking each one that you want to edit.

3. In the Details pane, under **Bulk Edit**, click **Update** under **Contact Information** or **Organization**.

4. Make the changes on the properties page and then save your changes.

### How do you know this worked?

To verify that you've successfully bulk edited mail contacts, do one of the following:

- In the EAC, select each of the mail contacts that you bulk edited, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif) to view the properties that you changed.

- In the Shell, use the **Get-Contact** cmdlet to verify the changes. For example, say you used the bulk edit feature in the EAC to change the manager and the office for all mail contacts from a vendor company named A. Datum Corporation. To verify these changes, you could run the following command in the Shell.

  ```powershell
  Get-Contact -ResultSize unlimited -Filter "Company -eq 'Adatum'" | Format-List Name,Office,Manager
  ```
