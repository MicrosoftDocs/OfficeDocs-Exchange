---
localization_priority: Normal
description: 'Summary: Learn how to get public folders configured and running in Exchange Server 2016 or Exchange Server 2019 for a new organization or in an organization that has never previously had public folders.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 7b419906-8977-47f0-8687-a87911b5ebec
ms.reviewer:
title: Set up public folders in a new organization
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Set up public folders in a new organization

Public folders in Exchange are based on a mailbox architecture that allows public folders to benefit from things such as the resiliency of a Database Availability Group (DAG) and other mailbox features.

For limits in on-premises Exchange Server, see [Limits for public folders](limits.md).

For additional management tasks related to public folders in Exchange Server, see [Public folder procedures](procedures.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 30 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in the [Sharing and collaboration permissions](../../permissions/feature-permissions/sharing-and-collaboration-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Create the primary public folder mailbox

The primary public folder mailbox contains a writeable copy of the public folder hierarchy plus content and is the first public folder mailbox that you create for your organization. Subsequent public folder mailboxes will be secondary public folder mailboxes, which will contain a read-only copy of the hierarchy plus content.

For detailed steps, see [Create a public folder mailbox](create-public-folder-mailboxes.md).

## Step 2: Create your first public folder

For detailed steps, see [Create a public folder](create-public-folders.md).

## Step 3: Assign permissions to the public folder
<a name="Perms"> </a>

After you create the public folder, you'll need to assign the **Owner** permissions level so that at least one user can access the public folder from the client and create subfolders. Any public folders created after this one will inherit the permissions of the parent public folder.

1. In the Exchange admin center (EAC), navigate to **Public folders** \> **Public folders**.

2. In the list view, select the public folder.

3. In the details pane, under **Folder permissions**, click **Manage**.

4. In **Public Folder Permissions**, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

5. Click **Browse** to select a user.

6. In the **Permission level** list, select a level. At least one user should be an **Owner**.

7. Click **Save**.

8. You can add multiple users by clicking **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png) and assigning the appropriate permissions using the steps above. You can also customize the permission level by selecting or clearing the check boxes. When you edit a predefined permission level such as **Owner**, the permission level will change to **Custom**.

For information about how to use the Exchange Management Shell to assign permissions to a public folder, see [Add-PublicFolderClientPermission](https://docs.microsoft.com/powershell/module/exchange/add-publicfolderclientpermission).

## Step 4 (Optional): Mail-enable the public folder
<a name="Perms"> </a>

If you want users to send mail to the public folder, you can mail-enable the public folder. This step is optional. If you don't mail-enable the public folder, users can post messages to the public folder by dragging into it items from Outlook.

1. In the EAC, navigate to **Public folders** \> **Public folders**.

2. In the list view, select the public folder you want to mail-enable.

3. In the details pane, under **Mail settings - Disabled**, click **Enable**.

    A warning displays asking if you're sure you want to enable mail for the public folder. Click **Yes**.

The public folder will be mail-enabled and the name of the public folder will become the alias of the public folder. If you have multiple recipients with that name, the public folder's alias will be appended with a number. For example, if you have a distribution group named SalesTeam and you create a public folder named SalesTeam and then mail-enable it, the alias of that public folder will be SalesTeam1.

For information about how to use the Exchange Management Shell to mail-enable a public folder, see [Enable-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/enable-mailpublicfolder).
