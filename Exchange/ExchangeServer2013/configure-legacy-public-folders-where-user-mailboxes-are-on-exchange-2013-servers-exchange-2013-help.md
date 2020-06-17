---
title: 'Configure legacy public folders when user mailboxes are on Exchange 2013 servers'
TOCTitle: Configure legacy public folders where user mailboxes are on Exchange 2013 servers
ms:assetid: 1d5ca19e-696e-4054-a634-15dd34d952b7
ms:mtpsurl: https://technet.microsoft.com/library/Dn690134(v=EXCHG.150)
ms:contentKeyID: 62380198
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure legacy public folders where user mailboxes are on Exchange 2013 servers

_**Applies to:** Exchange Server 2013_

How to enable Exchange 2013 or Exchange 2016 users to access Exchange 2010 or earlier public folders (also known as legacy public folders).

## What do you need to know before you begin?

Users whose mailboxes are on Exchange Server 2013 or Exchange Server 2016 won't be able to access legacy public folders from Outlook Web App, Outlook on the web, or Outlook for Mac. The steps in this article work for both Exchange 2013 and Exchange 2016.

> [!NOTE]
> Outlook 2016 for Mac users can access legacy public folders after you follow the steps in this article. If clients in your organization use Outlook 2016 for Mac, make sure they have installed the April 2016 update. Otherwise, those users will not be able to access public folders in a co-existence or hybrid topology. For more information, see <A href="https://docs.microsoft.com/exchange/collaboration-exo/public-folders/access-public-folders-with-outlook-2016-for-mac">Accessing public folders with Outlook 2016 for Mac</A>.

## Step 1: Make the Exchange 2010 public folders discoverable

1. If your public folders are on servers that are running Exchange Server 2010 or a later version, you must install the Client Access server (CAS) role on all mailbox servers that have a public folder database. You must also enable Outlook Anywhere on all public folder servers. This allows the Microsoft Exchange RpcClientAccess service to run so that all clients can access public folders. For Exchange Server 2007 public folder servers, the CAS role isn't required, and this step isn't necessary. For more information, see [Install Exchange Server 2010](install-exchange-2013-using-the-setup-wizard-exchange-2013-help.md).

    > [!NOTE]
    > This server doesn't have to be part of the Client Access load balancing. For more information, see <A href="https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ff625247(v=exchg.141)">Understanding Load Balancing in Exchange 2010</A>.

2. Create an empty mailbox database on each public folder server.

    For Exchange 2010, run the following command. This command excludes the mailbox database from the mailbox provisioning load balancer. This prevents new mailboxes from automatically being added to this database.

    ```powershell
    New-MailboxDatabase -Server <PFServerName_with_CASRole> -Name <NewMDBforPFs> -IsExcludedFromProvisioning $true
    ```

    For Exchange 2007, run the following command:

    ```powershell
    New-MailboxDatabase -StorageGroup "<PFServerName>\StorageGroup>" -Name <NewMDBforPFs>
    ```

    > [!NOTE]
    > We recommend that the only mailbox that you add to this database is the proxy mailbox that you'll create in step&nbsp;3. No other mailboxes should be created on this mailbox database.

3. Create a proxy mailbox within the new mailbox database and hide the mailbox from the address book. The SMTP of this mailbox will be returned by AutoDiscover as the *DefaultPublicFolderMailbox* SMTP, so that by resolving this SMTP the client can reach the legacy exchange server for public folder access.

    ```powershell
    New-Mailbox -Name <PFMailbox1> -Database <NewMDBforPFs> -EmailAddresses <email address> -UserPrincipalName <user principal name> -Password <password>
    ```

    ```powershell
    Set-Mailbox -Identity <PFMailbox1> -HiddenFromAddressListsEnabled $true
    ```

4. For Exchange 2010, enable AutoDiscover to return the proxy public folder mailboxes. This step isn't necessary for Exchange 2007.

    ```powershell
    Set-MailboxDatabase <NewMDBforPFs> -RPCClientAccessServer <PFServerName_with_CASRole>
    ```

5. Repeat the preceding steps for every public folder server in your organization.

## Step 2: Configure user mailboxes to access the legacy public folders

The final step in this procedure is to configure the user mailboxes to allow access to the legacy on-premises public folders.

Enable the Exchange Server 2013 on-premises users to access the legacy public folders. You will point to all of the proxy public folder mailboxes that you created in [Step 1: Make the Exchange 2010 public folders discoverable](https://docs.microsoft.com/exchange/collaboration-exo/public-folders/set-up-legacy-hybrid-public-folders). Run the following command from an Exchange 2013 server with the CU5 or higher update.

```powershell
Set-OrganizationConfig -PublicFoldersEnabled Remote -RemotePublicFolderMailboxes ProxyMailbox1,ProxyMailbox2,ProxyMailbox3
```

> [!NOTE]
> You must wait until ActiveDirectory synchronization has completed to see the changes. This process may take several hours.

## How do I know this worked?

Log on to Outlook for a user whose mailbox is on an Exchange Server 2013 CU5 or higher server and perform the following public folder tests:

1. Make sure that the Outlook client is running.

2. Hold down the CTRL key, and then right-click on the Outlook icon in the notification area on the right side of the Windows task bar.

3. Select **Test E-Mail Auto Configuration...**

4. Make sure the Text E-mail Auto Configuration tool returns the following information in the XML tab:

   - \<PublicFolderInformation\>

   - \<SmtpAddress\>\<SMTP Address for public folder mailbox\</SmtpAddress\>

   - \</PublicFolderInformation\>

5. In the Outlook client, peform the following tasks:

   - View the public folder hierarchy.

   - Check permissions.

   - Create and delete public folders.

   - Post content to and delete content from a public folder.
