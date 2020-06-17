---
title: 'Offline address books: Exchange 2013 Help'
TOCTitle: Offline address books
ms:assetid: a6bcb072-4ab9-400e-a5d0-c05264629097
ms:mtpsurl: https://technet.microsoft.com/library/Bb232155(v=EXCHG.150)
ms:contentKeyID: 49289363
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Offline address books

_**Applies to:** Exchange Server 2013_

An offline address book (OAB) is a copy of an address list collection that's been downloaded so a Microsoft Outlook user can access the address book while disconnected from the server. Microsoft Exchange generates the new OAB files and then compresses the files and places them on a local share. You can decide which address lists are made available to users who work offline, and you can also configure the method by which the address books are distributed.

To learn more about address lists, see [Address lists](https://docs.microsoft.com/exchange/address-books/address-lists/address-lists).

> [!IMPORTANT]
> OAB data is produced by the Microsoft Exchange OABGen service, which is a mailbox assistant. If you use the security descriptor to prevent users from viewing certain recipients in Active Directory, users who download the OAB will be able to view those hidden recipients. Therefore, to hide a recipient from an address list, set the <EM>HiddenFromAddressListsEnabled</EM> parameter on the <A href="https://docs.microsoft.com/powershell/module/exchange/Set-PublicFolder">Set-PublicFolder</A>, <A href="https://docs.microsoft.com/powershell/module/exchange/Set-MailContact)">Set-MailContact</A>, <A href="https://docs.microsoft.com/powershell/module/exchange/Set-MailUser">Set-MailUser</A>, <A href="https://docs.microsoft.com/powershell/module/exchange/Set-DynamicDistributionGroup">Set-DynamicDistributionGroup</A>, <A href="https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox">Set-Mailbox</A>, and <A href="https://docs.microsoft.com/powershell/module/exchange/Set-DistributionGroup">Set-DistributionGroup</A> cmdlets. Alternatively, you can create a new default OAB that doesn't contain the hidden recipients. For details about how to add or remove address lists from an OAB, see <A href="https://docs.microsoft.com/exchange/address-books/offline-address-books/add-or-remove-an-address-list">Add an address list to or remove an address list from an offline address book</A>.

Looking for management tasks related to OABs? See [Offline address book procedures](https://docs.microsoft.com/exchange/address-books/offline-address-books/offline-address-book-procedures).

## Moving OABs between Exchange versions

In Exchange 2007 and Exchange 2010, you use the **Move-OfflineAddressBook** cmdlet to move the OAB generation to another Mailbox server. Exchange 2013 supports only OAB (version 4). This is the same version that was the default in Exchange 2010. You can't configure Exchange 2013 to generate other OAB versions, and the OAB generation occurs on the Mailbox server on which the organization mailbox resides. Therefore, to move OAB generation in Exchange 2013, you must move the organization mailbox. You can only move the OAB generation to another Exchange 2013 mailbox database. You can't move OAB generation to a previous version of Exchange. To find the Exchange 2013 OAB organization mailbox, run the following Shell command:

```powershell
Get-Mailbox -Arbitration | where {$_.PersistedCapabilities -like "*oab*"}
```

You can then use the **MoveRequest** cmdlets to move the mailbox.

## OAB version 4 and Outlook clients

Exchange 2013 only supports OAB version 4. OAB version 4 was introduced in Exchange 2003 Service Pack 2 (SP2) and is supported by Outlook 2007, Outlook 2010, and Outlook 2013. This Unicode OAB allows client computers to receive differential updates rather than full OAB downloads and a reduced file size.

## Outlook clients that use OAB version 4

For Outlook 2013, Outlook 2010, Outlook 2007, and clients that use OAB version 4, if the size of the changes.oab files is half the size (or more) of the entire OAB files, Outlook initiates a full OAB download.

## Web-based distribution

*Web-based distribution* is the distribution method by which Outlook 2013, Outlook 2010, or Outlook 2007 clients that are working offline can access the OAB.

There are several advantages to using Web-based distribution, including:

- Support of more concurrent client computers.

- Reduction in bandwidth usage.

- More control over the OAB distribution points. With Web-based distribution, the distribution point is the HTTPS web address where client computers can download the OAB.

To benefit most from Web-based distribution, client computers must be running Outlook 2013, Outlook 2010, or Outlook 2007.

To function properly, Web-based distribution depends on the following components:

- **OAB generation process**: This is the process by which Exchange creates and updates the OAB. To create and update the OAB, the OABGen service runs on the Mailbox server on which the organization mailbox is located. To support OAB distribution, this server must be an Exchange Mailbox server.

- **OAB distribution**: If a client initiates the OAB distribution request, the request be directed through a Client Access server. The Client Access server then routes the request to the Mailbox server that's hosting the OAB files. The OAB files are then distributed directly from the Mailbox server to the client.

- **OAB virtual directory**: The OAB virtual directory is the distribution point used by the Web-based distribution method. By default, when Exchange is installed, a new virtual directory named **OAB** is created in the default internal website in Internet Information Services (IIS). If you have client-side users that connect to Outlook from outside your organization's firewall, you can add an external website. Alternatively, when you run the **New-OABVirtualDirectory** cmdlet in the Shell, a new virtual directory named OAB is created in the default IIS website on the local Exchange Client Access server. For information, see [Create an offline address book virtual directory](https://docs.microsoft.com/exchange/address-books/offline-address-books/create-virtual-directory).

- **Autodiscover service**: This is a feature available in Outlook 2013, Outlook 2010, Outlook 2007, and in some mobile devices that automatically configure the clients for access to Exchange. The service runs on a Client Access server and returns the correct OAB URL for a specific client connection.

## OAB considerations

As a best practice, whether you use a single OAB or multiple OABs, consider the following factors as you plan and implement your OAB strategy:

- Size of each OAB in your organization. For more information, see OAB size considerations later in this topic.

- Number of OAB downloads.

- Number and frequency of parent distinguished name changes.

- SMTP address mismatches.

- Overall number of changes made to the directory.

## OAB size considerations

For some organizations, the OAB is a small file that remote users occasionally download. For these organizations, downloading the OAB is usually not a concern. However, for some large organizations that have large directories, or for organizations that have deployed Outlook 2003 in Cached Exchange Mode, it may be a concern, especially if the organizations have consolidated Exchange servers into a regional datacenter.

OAB sizes can vary from a few megabytes to a few hundred megabytes. The following factors can affect the size of the OAB:

- Usage of certificates in a company. The more public key infrastructure (PKI) certificates, the larger the OAB. PKI certificates range from 1 kilobyte (KB) to 3 KB. They're the single largest contributor to the OAB size.

- Number of mail recipients in Active Directory.

- Number of distribution groups in Active Directory.

- Information that a company adds to Active Directory for each mailbox-enabled or mail-enabled object. For example, some organizations populate the address properties on each user; others don't.
