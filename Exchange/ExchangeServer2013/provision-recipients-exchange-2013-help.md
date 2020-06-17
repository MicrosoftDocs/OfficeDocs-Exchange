---
title: 'Provision recipients for offline address book downloads: Exchange 2013 Help'
TOCTitle: Provision recipients for offline address book downloads
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 141751ac-16d3-4e3c-b70c-004aeedcb5a0
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Provision recipients for offline address book downloads in Exchange 2013

_**Applies to:** Exchange Server 2013_

If you use multiple offline address books (OABs) in your organization, there are several ways to specify which recipients download which OABs:

- **Per mailbox database** You can use the EAC or the Shell to provision recipients for OAB downloads by linking a mailbox database to a default OAB for Office Outlook 2007, Outlook 2010 and Outlook 2013 clients.

- **Per recipient** You can use the **Set-Mailbox** cmdlet in the Shell to specify which OAB is downloaded by linking the OAB directly to a recipient's mailbox.

- **Per multiple recipients** You can use a pipelined command in the Shell to specify the OAB that multiple recipients download, based on common attributes.

- **Per address book policy** You can assign an address book policy (ABP) to a mailbox user's account to specify which OAB is downloaded to a recipient's mailbox. If you assign an ABP to a user account that already has an OAB assigned, the OAB that's explicitly assigned to the mailbox will take precedence. For more information, see [Assign an address book policy to mail users](assign-an-address-book-policy-to-mail-users-exchange-2013-help.md).

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You can't use the Exchange admin center (EAC) to perform these procedures. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to provision recipients for OAB downloads by linking their mailbox database to a public folder database or to a default OAB

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox databases" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

This example sets up the web-based distribution of My OAB for the default mailbox database.

```powershell
Set-MailboxDatabase -Identity "Mailbox Database" -OfflineAddressBook "My OAB"
```

For detailed syntax and parameter information, see [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabase).

## Use the Shell to specify which OAB will be downloaded by linking the OAB directly to a recipient's mailbox

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

To specify which OAB is downloaded by linking the OAB directly to a recipient's mailbox, use the following syntax.

```powershell
Set-Mailbox -Identity <MailboxIDParameter> -OfflineAddressBook <OfflineAddressBookIdParameter>
```

> [!NOTE]
> The _Identity_ parameter identifies the mailbox and can take the following values: GUID, ADObjectID, distinguished name (DN), _domain\account_, user principal name (UPN), LegacyExchangeDN, SmtpAddress, and alias.

This example specifies that the user Kim will download the OAB My OAB.

```powershell
Set-Mailbox -Identity Kim -OfflineAddressBook "My OAB"
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

## Use the Shell to specify the OAB that multiple recipients will download

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

This example specifies that all user mailboxes in the United States for Contoso will download the OAB Contoso United States.

```powershell
Get-User -ResultSize Unlimited -Filter "Company -eq 'Contoso' -and RecipientType -eq 'UserMailbox'" | Where { $_.CountryOrRegion -eq "United States"} | Set-Mailbox -OfflineAddressBook "Contoso United States"
```

For detailed syntax and parameter information, see [Get-User](https://docs.microsoft.com/powershell/module/exchange/get-user) and [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).
