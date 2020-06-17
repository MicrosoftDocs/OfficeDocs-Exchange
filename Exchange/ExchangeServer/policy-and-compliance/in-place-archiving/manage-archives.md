---
localization_priority: Normal
description: 'Summary: Learn how to enable, disable, and re-enable archive mailboxes in Exchange Server, and how to verify the archive mailbox settings for a user.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 49ef4a3e-d209-4fb2-80a3-6132b0f69bd0
ms.reviewer:
title: Manage In-Place Archives in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Manage In-Place Archives in Exchange Server

In-Place Archiving helps you regain control of your organization's messaging data by eliminating the need for personal store (.pst) files and allowing you to meet your organization's message retention and eDiscovery requirements. With archiving enabled, users can store messages in an archive mailbox, which is accessible by using Microsoft Outlook and Outlook on the web.

## What do you need to know before you begin?

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place Archive" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

- The procedures in this topic apply to on-premises archive mailboxes. For information about archive mailboxes in Exchange Online, see [Enable archive mailboxes in the Security & Compliance Center](https://docs.microsoft.com/microsoft-365/compliance/enable-archive-mailboxes).

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- It's not supported to have a user's primary mailbox reside on a version of Exchange that's older than the user's archive. If the user's primary mailbox is still on Exchange 2010 or Exchange 2013, you need to move it to Exchange 2016 or Exchange 2019 at the same time you move the archive mailbox to Exchange 2016 or Exchange 2019.

## Enable an archive mailbox

You can use the Exchange admin center or the Exchange Management Shell to enable archive mailboxes for users that already have a primary mailbox.

### Use the EAC to enable an archive mailbox

1. Go to **Recipients** \> **Mailboxes**.

2. Select a mailbox.

3. In the details pane, under **In-Place Archive**, click **Enable**.

   **Note**: You can also bulk-enable archives by selecting multiple mailboxes (use the Shift or Ctrl keys). After selecting multiple mailboxes, in the details pane, click **More options**. Then, under **Archive** click **Enable**.

4. On the **Create In-Place Archive** page, click **OK** to have Exchange automatically select a mailbox database for the archive or click **Browse** to specify one.

### Use the Exchange Management Shell to enable an archive mailbox

This example enables the archive mailbox for Tony Smith.

```PowerShell
Enable-Mailbox "Tony Smith" -Archive
```

This example retrieves mailboxes in database DB01 that don't have an on-premises or cloud-based archive enabled and don't have a name starting with DiscoverySearchMailbox. It pipes the results to the **Enable-Mailbox** cmdlet to enable the archive for all mailboxes on mailbox database DB01.

```PowerShell
Get-Mailbox -Database DB01 -Filter "ArchiveGuid -Eq `$null -AND ArchiveDomain -eq `$null -AND Name -NotLike 'DiscoverySearchMailbox*'" | Enable-Mailbox -Archive
```

### How do you know this worked?

To verify that you've successfully enabled an on-premises archive for an existing mailbox, do one of the following:

- In the EAC, go to **Recipients** \> **Mailboxes**, and then select the mailbox from the list. In the details pane, under **In-Place Archive**, confirm that it is set to **Enabled**. Click **View details** to view archive properties, including archive status and the mailbox database in which it is created.

- In the Exchange Management Shell, run the following command to display information about the new archive.

  ```PowerShell
  Get-Mailbox <MailboxIdentity> | Format-List Name,*Archive*
  ```

- In the Exchange Management Shell, use the **Test-ArchiveConnectivity** cmdlet to test connectivity to the archive. For an example of how to test archive connectivity, see the Examples section in the topic, [Test-ArchiveConnectivity](https://docs.microsoft.com/powershell/module/exchange/test-archiveconnectivity).

## Enable an archive mailbox when you create a new mailbox

You can also enable an archive mailbox when you first create a new mailbox for a user.

### Use the EAC to enable an archive mailbox when you create a new mailbox

1. Go to **Recipients** \> **Mailboxes**.

2. Click **New** \> **User mailbox**.

3. On the **New user mailbox** page, in the **Alias** box, type an alias for the user.

   **Note**: If you leave this box blank, the value you type in the **User logon name** box is used for the alias.

4. Select one of the following options:

   - **Existing user that isn't mail-enabled**: Click this button and then click **Browse** to open the **Select User - Entire Forest** dialog box. This dialog box displays a list of Active Directory user accounts in the forest that aren't mail-enabled or don't have Exchange mailboxes. Select the user account you want to mail-enable, and then click **OK**. If you select this option, you don't have to provide user account information because this information already exists in Active Directory.

   - **New user**: Click this button to create a new user account in Active Directory and create a mailbox for the user. If you select this option, you'll have to provide the required user account information.

5. Click **More options** to configure the following settings.

   - **Mailbox database**: Click **Browse** to select a mailbox database in which to store the mailbox. If you don't select a database, Exchange will automatically assign one.

   - **Archive**: Select this check box to create an archive mailbox for the mailbox. If you create an archive mailbox, mailbox items will be moved automatically from the primary mailbox to the archive, based on the default retention policy settings or those you define.

   Click **Browse** to select a database to store the archive mailbox.

6. When you're finished, click **Save** to create the mailbox and its archive.

### Use the Exchange Management Shell to enable an archive mailbox when you create a new mailbox

This example creates the user named Chris Ashton in Active Directory, creates the mailbox on mailbox database DB01, and enables and creates an archive mailbox on DB01. To set the initial value of the password, this example creates a variable ($password), prompts you to enter a password, and assigns that password to the variable as a SecureString object.

```PowerShell
$password = Read-Host "Enter password" -AsSecureString
```

```PowerShell
New-Mailbox -UserPrincipalName cashton@contoso.com -Alias cashton -Database "DB01" -Archive -Name "Chris Ashton" -OrganizationalUnit Users -Password $password -FirstName Chris -LastName Ashton
```

### How do you know this worked?

To verify that you've successfully created a user mailbox with an on-premises archive, do one of the following:

- In the EAC, go to **Recipients** \> **Mailboxes**, and then select the new user mailbox from the list. In the details pane, under **In-Place Archive**, confirm that it is set to **Enabled**. Click **View details** to view archive properties, including archive status and the mailbox database in which it is created.

- In the Exchange Management Shell, run the following command to display information about the new user mailbox and archive.

  ```PowerShell
  Get-Mailbox <Name> | Format-List Name,RecipientTypeDetails,PrimarySmtpAddress,*Archive*
  ```

- In the Exchange Management Shell, use the **Test-ArchiveConnectivity** cmdlet to test connectivity to the archive. For an example of how to test archive connectivity, see the Examples section in [Test-ArchiveConnectivity](https://docs.microsoft.com/powershell/module/exchange/test-archiveconnectivity).

## Disable an archive mailbox

You may want to disable a user's archive for troubleshooting purposes or compliance-related reasons. If you disable an archive mailbox, all information in the archive will be kept in the mailbox database until the mailbox retention time expires and the archive is permanently deleted. By default, Exchange keeps deleted mailboxes, including archive mailboxes, for 30 days.

### Use the EAC to disable an archive mailbox

1. Go to **Recipients** \> **Mailboxes**.

2. Select a mailbox.

3. In the details pane, under **In-Place Archive**, click **Disable**.

   **Note**: You can also bulk-disable archives by selecting multiple mailboxes (use the Shift or Ctrl keys). After selecting multiple mailboxes, in the details pane, click **More options**. Then, under **Archive** click **Disable**.

### Use the Exchange Management Shell to disable an archive mailbox

This example disables the archive mailbox for Chris Ashton's mailbox. It doesn't disable the user's primary mailbox.

```PowerShell
Disable-Mailbox "Chris Ashton" -Archive
```

### How do you know this worked?

To verify that you have successfully disabled an archive mailbox, do the following:

- In the EAC, select the mailbox. In the details pane, check its archive status under **In-Place Archive**.

- In the Exchange Management Shell, run the following command to check the archive properties for the mailbox user.

  ```PowerShell
  Get-Mailbox "Chris Ashton" | Format-List *Archive*
  ```

If the archive is disabled, the following values are returned for archive-related properties.

|**Property**|**Value**|
|:-----|:-----|
|**ArchiveDatabase** (for on-premises archives)|\<blank\>|
|**ArchiveState**| `None`|
|**DisabledArchiveDatabase** (for on-premises archives)| _\<name of mailbox database\>_|
|**DisabledArchiveGuid**| _\<GUID of disabled archive\>_|

## Re-enable an archive mailbox

When you disable an archive mailbox, it becomes disconnected. A disconnected archive mailbox is retained in the mailbox database for a specified amount of time. By default, Exchange retains disconnected archive mailboxes for 30 days. Within 30 days of disabling an archive mailbox, you can reconnect it to the user's primary mailbox by re-enabling the archive. In this case, the original contents of the archive mailbox are restored. However after 30 days of disabling a mailbox, the contents of the original archive mailbox are permanently deleted (purged from the mailbox database) and can't be recovered. So if you re-enable the archive more than 30 days after disabling it, a new archive mailbox is created when you re-enable it.

### Use the EAC to re-enable an archive mailbox

1. Go to **Recipients** \> **Mailboxes**.

2. Select the mailbox.

3. In the details pane, under **In-Place Archive**, click **Enable**

4. On the **Create in-place archive** page, click **OK**.

   You can have Exchange automatically select a mailbox database for the re-enabled archive mailbox or you can click **Browse** to specify one.

### Use the Exchange Management Shell to re-enable an archive mailbox

Use the **Enable-Mailbox -Archive** command to re-enable an archive mailbox. For example:

```PowerShell
Enable-Mailbox "Chris Ashton" -Archive
```

### How do you know this worked?

To verify that you have successfully connected a disabled archive mailbox to the user's primary mailbox, run the following command to retrieve the mailbox user's archive properties, and verify the values returned for the _ArchiveGuid_ and _ArchiveDatabase_ properties.

```PowerShell
Get-Mailbox "Chris Ashton" | Format-List *Archive*
```

As previously stated, if you re-enable an archive mailbox within 30 days of disabling it, the user will be able to access the original contents of their archive mailbox. If you re-enable the archive more than 30 days after disabling it, the new archive mailbox will be empty the first time the user accesses it.
