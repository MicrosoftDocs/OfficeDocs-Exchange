---
title: 'Manage In-Place Archives in Exchange 2013: Exchange 2013 Help'
TOCTitle: Manage In-Place Archives in Exchange 2013
ms:assetid: 49ef4a3e-d209-4fb2-80a3-6132b0f69bd0
ms:mtpsurl: https://technet.microsoft.com/library/JJ651146(v=EXCHG.150)
ms:contentKeyID: 49352793
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage In-Place Archives in Exchange 2013

_**Applies to:** Exchange Server 2013_

In-Place Archiving helps you regain control of your organization's messaging data by eliminating the need for personal store (.pst) files and allowing you to meet your organization's message retention and eDiscovery requirements. With archiving enabled, users can store messages in an archive mailbox, which is accessible by using Microsoft Outlook and Outlook Web App.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place Archive" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- It's not supported to have a user's primary mailbox reside on an older Exchange version than the user's archive. If the user's primary mailbox is still on Exchange 2010, you must move it to Exchange 2013 at the same time you move the archive to Exchange 2013.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a mailbox and enable an on-premises archive

## Use the EAC

1. Navigate to **Recipients** \> **Mailboxes**.

2. Click **New** \> **User mailbox**.

3. On the **New user mailbox** page, in the **Alias** box, type an alias for the user.

    > [!NOTE]
    > If you leave this box blank, the value you type in the <STRONG>User logon name</STRONG> box is used for the alias.

4. Select one of the following options:

      - **Existing user**: Click this button and then click **Browse** to open the **Select User - Entire Forest** dialog box. This dialog box displays a list of Active Directory user accounts in the forest that aren't mail-enabled or don't have Exchange mailboxes. Select the user account you want to mail-enable, and then click **OK**. If you select this option, you don't have to provide user account information because this information already exists in Active Directory.

      - **New user**: Click this button to create a new user account in Active Directory and create a mailbox for the user. If you select this option, you'll have to provide the required user account information.

    > [!NOTE]
    > The Active Directory account that is associated with user mailboxes must reside in the same forest as the Exchange server. To create a mailbox for a user account that resides in a trusted forest, you have to create a linked mailbox. For details, see <A href="manage-linked-mailboxes-exchange-2013-help.md">Manage linked mailboxes</A>.

5. Click **More options** to configure the following settings.

      - **Mailbox database**: Click **Browse** to select a mailbox database in which to store the mailbox. If you don't select a database, Exchange will automatically assign one.

      - **Archive**: Select this check box to create an archive mailbox for the mailbox. If you create an archive mailbox, mailbox items will be moved automatically from the primary mailbox to the archive, based on the default retention policy settings or those you define.

        Click **Browse** to select a database that resides in the local forest to store the archive mailbox.

        To learn more, see [In-Place Archiving in Exchange 2013](in-place-archiving-in-exchange-2013-exchange-2013-help.md).

      - **Address book policy**: Use this list to select an address book policy (ABP) for the mailbox. ABPs contain a global address list (GAL), an offline address book (OAB), a room list, and a set of address lists. When assigned to mailbox users, an ABP provides them with access to a customized GAL in Outlook and Outlook Web App. To learn more, see [Address book policies](https://docs.microsoft.com/exchange/address-books/address-book-policies/address-book-policies).

6. When you're finished, click **Save** to create the mailbox.

## Use the Shell

This example creates the user Chris Ashton in Active Directory, creates the mailbox on mailbox database DB01, and enables an archive. The password must be reset at the next logon. To set the initial value of the password, this example creates a variable ($password), prompts you to enter a password, and assigns that password to the variable as a SecureString object.

```powershell
$password = Read-Host "Enter password" -AsSecureString
New-Mailbox -UserPrincipalName chris@contoso.com -Alias chris -Archive -Database "DB01" -Name ChrisAshton -OrganizationalUnit Users -Password $password -FirstName Chris -LastName Ashton -DisplayName "Chris Ashton"
```

For detailed syntax and parameter information, see [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/New-Mailbox).

## How do you know this worked?

To verify that you've successfully created a user mailbox with an on-premises archive, do one of the following:

- In the EAC, navigate to **Recipients**  \> **Mailboxes**, and then select the new user mailbox from the list. In the details pane, under **In-Place Archive**, confirm that it is set to **Enabled**. Click **View details** to view archive properties, including archive status and the mailbox database in which it is created.

- In the Shell, run the following command to display information about the new user mailbox and archive.

    ```powershell
    Get-Mailbox <Name> | FL Name,RecipientTypeDetails,PrimarySmtpAddress,*Archive*
    ```

- In the Shell, use the **Test-ArchiveConnectivity** cmdlet to test connectivity to the archive. For an example of how to test archive connectivity, see the Examples section in [Test-ArchiveConnectivity](https://docs.microsoft.com/powershell/module/exchange/Test-ArchiveConnectivity).

## Enable an on-premises archive for existing mailbox

You can also create archives for existing users that have a mailbox but aren't archive-enabled. This is known as *enabling an archive* for an existing mailbox.

## Use the EAC

1. Navigate to **Recipients**  \> **Mailboxes**.

2. Select a mailbox.

3. In the details pane, under **In-Place Archive**, click **Enable**

    > [!TIP]
    > You can also bulk-enable archives by selecting multiple mailboxes (use the Shift or Ctrl keys). After selecting multiple mailboxes, in the details pane, click <STRONG>More options</STRONG>. Then, under <STRONG>Archive</STRONG> click <STRONG>Enable</STRONG>.

4. On the **Create in-place archive** page, click **OK** to have Exchange automatically select a mailbox database for the archive or click **Browse** to specify one.

## Use the Shell

This example enables the archive for Tony Smith's mailbox.

```powershell
Enable-Mailbox "Tony Smith" -Archive
```

This example retrieves mailboxes in database DB01 that don't have an on-premises or cloud-based archive enabled and don't have a name starting with DiscoverySearchMailbox. It pipes the result to the **Enable-Mailbox** cmdlet to enable the archive for all mailboxes on mailbox database DB01.

```powershell
Get-Mailbox -Database DB01 -Filter "ArchiveGuid -Eq `$null -AND ArchiveDomain -eq `$null -AND Name -NotLike 'DiscoverySearchMailbox*'" | Enable-Mailbox -Archive
```

For detailed syntax and parameter information, see [Enable-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Enable-Mailbox) and [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox).

## How do you know this worked?

To verify that you've successfully enabled an on-premises archive for an existing mailbox, do one of the following:

- In the EAC, navigate to **Recipients**  \> **Mailboxes**, and then select the mailbox from the list. In the details pane, under **In-Place Archive**, confirm that it is set to **Enabled**. Click **View details** to view archive properties, including archive status and the mailbox database in which it is created.

- In the Shell, run the following command to display information about the new archive.

    ```powershell
    Get-Mailbox <Name> | FL Name,*Archive*
    ```

- In the Shell, use the **Test-ArchiveConnectivity** cmdlet to test connectivity to the archive. For an example of how to test archive connectivity, see Examples in [Test-ArchiveConnectivity](https://docs.microsoft.com/powershell/module/exchange/Test-ArchiveConnectivity).

## Disable an on-premises archive

You may want to disable a user's archive for troubleshooting purposes or if you're moving the mailbox to a version of Exchange that doesn't support In-Place Archiving. If you disable an on-premises archive, all information in the archive will be kept in the mailbox database until the mailbox retention time passes and the archive is permanently deleted. (By default, Exchange keeps disconnected mailboxes, including archive mailboxes, for thirty days.)

> [!IMPORTANT]
> Disabling the archive will remove the archive from the mailbox and mark it in the mailbox database for deletion.

If you want to reconnect the on-premises archive to that mailbox, you can use the [Connect-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Connect-Mailbox) cmdlet with the *Archive* parameter.

## Use the EAC

1. Navigate to **Recipients**  \> **Mailboxes**.

2. Select a mailbox.

3. In the details pane, under **In-Place Archive**, click **Disable**.

    > [!TIP]
    > You can also bulk-disable archives by selecting multiple mailboxes (use the Shift or Ctrl keys). After selecting multiple mailboxes, in the details pane, click <STRONG>More options</STRONG>. Then, under <STRONG>Archive</STRONG> click <STRONG>Disable</STRONG>.

## Use the Shell

This example disables the archive for Chris Ashton's mailbox. It doesn't disable the mailbox.

```powershell
Disable-Mailbox -Identity "Chris Ashton" -Archive
```

For detailed syntax and parameter information, see [Disable-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Disable-Mailbox).

## How do you know this worked?

To verify that you have successfully disabled an archive, do the following:

- In the EAC, select the mailbox. Then, in the details pane check its archive status under **In-Place Archive**.

- In the Shell, run the following command to check the archive properties for the mailbox user.

    ```powershell
    Get-Mailbox -Identity "Chris Ashton" | Format-List *Archive*
    ```

    If the archive is disabled, the following values are returned for archive-related properties.

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Property</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>ArchiveDatabase (for on-premises archives)</p></td>
    <td><p>&lt;blank&gt;</p></td>
    </tr>
    <tr class="even">
    <td><p>ArchiveState</p></td>
    <td><p><code>None</code></p></td>
    </tr>
    <tr class="odd">
    <td><p>DisabledArchiveDatabase (for on-premises archives)</p></td>
    <td><p>&lt;name of mailbox database&gt;</p></td>
    </tr>
    <tr class="even">
    <td><p>DisabledArchiveGuid</p></td>
    <td><p>&lt;guid of disabled archive&gt;</p></td>
    </tr>
    </tbody>
    </table>

## Connect an on-premises archive

When you disable an archive mailbox, it becomes disconnected. A disconnected archive mailbox is retained in the mailbox database for a specified amount of time. By default, Exchange retains disconnected archives for 30 days. During this time, you can recover the archive by associating it with an existing mailbox. You can modify the deleted mailbox retention period to retain a deleted mailbox or archive for a longer or shorter period.

> [!WARNING]
> If you disable an archive for a user and then enable an archive for that same user, the user will get a new archive. The new archive won't contain the data that was in the user's disconnected archive. If you want to reconnect a user to his or her disconnected archive, you must perform this procedure.

> [!NOTE]
> You can't use the EAC to connect a disconnected archive to a mailbox user.

## Use the Shell

1. If you don't know the name of the archive, you can view it in the Shell by running the following command. This example retrieves the mailbox database DB01, pipes it to the **Get-MailboxStatistics** cmdlet to retrieve mailbox statistics for all mailboxes on the database, and then uses the **Where-Object** cmdlet to filter the results and retrieve a list of disconnected archives. The command displays additional information about each archive such as the GUID and item count.

    ```powershell
    Get-MailboxDatabase "DB01" | Get-MailboxStatistics | Where {($_.DisconnectDate -ne $null) -and ($_.IsArchiveMailbox -eq $true)} | Format-List
    ```

2. Connect the archive to the primary mailbox. This example connects Chris Ashton's archive to Chris Ashton's primary mailbox and uses the GUID as the archive's identity.

    ```powershell
    Enable-Mailbox -ArchiveGuid "8734c04e-981e-4ccf-a547-1c1ac7ebf3e2" -ArchiveDatabase "DB01" -Identity "Chris Ashton"
    ```

For detailed syntax and parameter information, see the following topics:

- [Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxDatabase)

- [Get-MailboxStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxStatistics)

- [Enable-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Enable-Mailbox)

## How do you know this worked?

To verify that you have successfully connected a disconnected archive to a mailbox user, run the following Shell command to retrieve the mailbox user's archive properties and verify the values returned for the *ArchiveGuid* and *ArchiveDatabase* properties.:

```powershell
Get-Mailbox -Identity "Chris Ashton" | Format-List *Archive*
```
