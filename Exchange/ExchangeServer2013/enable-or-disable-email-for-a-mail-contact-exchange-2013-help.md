---
title: 'Enable or disable email for a mail contact: Exchange 2013 Help'
TOCTitle: Enable or disable email for a mail contact
ms:assetid: ca47441f-1aa4-4958-aba5-18d51e59837e
ms:mtpsurl: https://technet.microsoft.com/library/Bb124552(v=EXCHG.150)
ms:contentKeyID: 50383001
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable email for a mail contact

_**Applies to:** Exchange Server 2013_

You can disable email for an existing mail contact in your Exchange organization. When you disable email for a mail contact, it's removed from Exchange and from your organization's address book. If the mail contact is a member of a distribution group, the contact no longer receives mail sent to the group. Also, the Exchange attributes are removed from the mail-enabled contact object in Active Directory, but the contact and its non-Exchange attributes (such as contact and organization information) are retained in Active Directory.

After you disable email for a mail contact, you can mail-enable the contact again by using the **Enable-MailContact** cmdlet in the Shell. You can also use this cmdlet to mail-enable any Active Directory contact.

For additional management tasks related to mail contacts, see [Manage mail contacts](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-contacts).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail contacts" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Disable email for a mail contact

As previously stated, when you disable email for a mail contact, the Exchange attributes are removed from the corresponding Active Directory contact object, but the contact is retained. The mail contact is removed from the list of mail contacts in the EAC, but you can view and manage the corresponding Active Directory contact object by using Active Directory Users and Computers or by using the **Get-Contact** and **Set-Contact** cmdlets in the Shell.

## Use the EAC to disable email for a mail contact

1. In the EAC, navigate to **Recipients**  \> **Contacts**.

2. In the list of contacts, click the mail contact for which you want to disable email.

3. Click **More** ![More Options Icon](images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif "More Options Icon") and then click **Disable**.

4. A warning will appear asking if you're sure you want to disable the selected mail contact. Click **Yes** to disable it.

The mail contact will be removed from the contacts list.

## Use the Shell to disable email for a mail contact

This example disables email for the mail contact Neil Black.

```powershell
Disable-MailContact -Identity "Neil Black"
```

For detailed syntax and parameter information, see [Disable-MailContact](https://docs.microsoft.com/powershell/module/exchange/Disable-MailContact).

## How do you know this worked?

To verify that you've successfully disabled email for a mail contact, do one of the following:

1. In the EAC, navigate to **Recipients** \> **Contacts** and verify that the mail contact is no longer listed.

2. In Active Directory Users and Computers, right-click the contact, and then click **Properties**. On the **General** tab, notice that the **E-mail** box is blank. This verifies that the contact isn't mail-enabled.

3. In the Shell, run the following command.

    ```powershell
    Get-MailContact
    ```

    The contact that you disabled email for won't be returned in the results because this cmdlet only returns mail-enabled contacts.

4. In the Shell, run the following command.

    ```powershell
    Get-Contact
    ```

    The contact that you disabled email for is returned in the results because this cmdlet returns all Active Directory contact objects.

## Use the Shell to mail-enable contacts

You can use the **Enable-MailContact** cmdlet to mail-enable existing Active Directory contacts. You can mail-enable a single contact or use a CSV file to mail-enable multiple contacts.

## Use the Shell to mail-enable a single contact

This example mail-enables the contact Rene Valdes. You must provide an external email address.

```powershell
Enable-MailContact -Identity "Rene Valdes" -ExternalEmailAddress renev@tailspintoys.com
```

## Use the Shell and a CSV file to mail-enable multiple contacts

When you're mail-enabling contacts in bulk, you first export the list of contacts that aren't mail-enabled to a CSV (comma-separated values) file, and then add the external email addresses to the CSV file by using a text editor such as Notepad, or a spreadsheet application such as Microsoft Excel. Then you use the updated CSV file in the Shell command to mail-enable the contacts listed in the CSV file.

1. Run the following command to export a list of existing contacts that aren't mail-enabled to a file on the administrator's desktop named Contacts.csv.

    ```powershell
    Get-Contact | Where {$_.RecipientType -eq "Contact"} | Out-File "C:\Users\Administrator\Desktop\Contacts.csv"
    ```

    The resulting file will be similar to the following file.

    ```powershell
    Name
    Walter Harp
    James Alvord
    Rainer Witt
    Susan Burk
    Ian Tien
    ...
    ```

2. Add a column heading named **EmailAddress** and then add an email address for each contact in the file. The name and external email address for each contact must be separated by a comma. The updated CSV file should look similar to the following file.

    ```powershell
    Name,EmailAddress
    James Alvord,james@contoso.com
    Susan Burk,sburk@tailspintoys.com
    Walter Harp,wharp@tailspintoys.com
    Ian Tien,iant@tailspintoys.com
    Rainer Witt,rainerw@fourthcoffee.com
    ...
    ```

3. Run the following command to use the data in the CSV file to mail-enable the contacts listed in the file.

    ```powershell
    Import-CSV C:\Users\Administrator\Desktop\Contacts.csv | ForEach-Object {Enable-MailContact -Identity $_.Name -ExternalEmailAddress $_.EmailAddress}
    ```

    The command results display information about the new mail-enabled contacts.

## How do you know this worked?

To verify that you've successfully mail-enabled Active Directory contacts, do one of the following:

- In the EAC, navigate to **Recipients** \> **Contacts**. New mail contacts are displayed in the contact list. Under **Contact Type**, the type is **Mail contact**.

    > [!NOTE]
    > You may have to click <STRONG>Refresh</STRONG> <IMG title="Refresh Icon" alt="Refresh Icon" src="images/Dn624163.85f271ca-32a4-426c-842a-d2172567099d(EXCHG.150).gif"> to display new mail contacts.

- In the Shell, run the following command to display information about new mail contacts.

    ```powershell
    Get-MailContact | Format-Table Name,RecipientTypeDetails,ExternalEmailAddress
    ```
