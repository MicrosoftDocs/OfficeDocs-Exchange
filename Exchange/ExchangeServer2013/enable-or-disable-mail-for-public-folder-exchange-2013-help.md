---
title: 'Mail-enable or mail-disable a public folder: Exchange 2013 Help'
TOCTitle: Mail-enable or mail-disable a public folder
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 3d69f76d-ff3c-46c1-b962-6a1baa425d8a
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Mail-enable or mail-disable a public folder in Exchange 2013

_**Applies to:** Exchange Server 2013_

Public folders are designed for shared access and provide an easy and effective way to collect, organize, and share information with other people in your workgroup or organization. Mail-enabling a public folder allows users to post to the public folder by sending an email message to it. When a public folder is mail-enabled additional settings become available for the public folder in the Exchange admin center (EAC), such as email addresses and mail quotas. In the Shell, before a public folder is mail-enabled, you use the **Set-PublicFolder** cmdlet to manage all of its settings. After the public folder is mail-enabled, you use the **Set-PublicFolder** and the **Set-MailPublicFolder** cmdlets to manage the settings.

If you want users on the Internet to send mail to a mail-enabled public folder, you need to set addition permissions using the **Add-PublicFolderClientPermission** cmdlet.

For additional management tasks related to managing public folders, see [Public folder procedures](public-folder-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- To ensure that users on the Internet can send e-mail messages to a mail-enabled public folder, the public folder needs to have at least the _CreateItems_ access right granted to the Anonymous account. If you want to learn how to do this, check out the [Allow anonymous users to send email to a mail-enabled public folder](#allow-anonymous-users-to-send-email-to-a-mail-enabled-public-folder) section later in this topic.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in the [Sharing and collaboration permissions](sharing-and-collaboration-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to mail-enable or mail-disable a public folder

1. Navigate to **Public folders** \> **Public folders**.

2. In the list view, select the public folder that you want to mail-enable or mail-disable.

3. In the details pane, under **Mail settings**, click **Enable** or **Disable**.

4. A warning box displays asking if you are sure you want to enable or disable email for the public folder. Click **Yes** to continue.

If you want external users to send mail to this public folder, make sure you follow the steps in the [Allow anonymous users to send email to a mail-enabled public folder](#allow-anonymous-users-to-send-email-to-a-mail-enabled-public-folder) section later in this topic.

## Use the Shell to mail-enable a public folder

This example mail-enables the public folder Help Desk.

```powershell
Enable-MailPublicFolder -Identity "\Help Desk"
```

This example mail-enables the public folder Reports under the Marketing public folder, but hides the folder from address lists.

```powershell
Enable-MailPublicFolder -Identity "\Marketing\Reports" -HiddenFromAddressListsEnabled $True
```

If you want external users to send mail to this public folder, make sure you follow the steps in the [Allow anonymous users to send email to a mail-enabled public folder](#allow-anonymous-users-to-send-email-to-a-mail-enabled-public-folder) section later in this topic.

For detailed syntax and parameter information, see [Enable-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/enable-mailpublicfolder).

## Use the Shell to mail-disable a public folder

This example mail-disables the public folder Marketing\Reports.

```powershell
Disable-MailPublicFolder -Identity "\Marketing\Reports"
```

For detailed syntax and parameter information, see [Disable-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/disable-mailpublicfolder).

## Allow anonymous users to send email to a mail-enabled public folder

You can use either Outlook or the Shell to set permissions on a public folder's Anonymous account. You can't use the EAC to set permissions on the Anonymous account.

### Use Outlook to set permissions for the Anonymous account

1. Open Outlook using an account that's been granted Owner permissions on the email-enabled public folder you want anonymous users to send mail to.

2. Navigate to **Public folders - \<user's name\>**.

3. Navigate to the public folder you want to change.

4. Right-click on the public folder, click **Properties** and then select the **Permissions** tab.

5. Select the **Anonymous** account, select **Create items** under **Write**, and then click **OK**.

### Use the Shell to set permissions for the Anonymous account

This example sets the `CreateItems` permission for the Anonymous account on the "Customer Feedback" mail-enabled public folder.

```powershell
Add-PublicFolderClientPermission "\Customer Feedback" -AccessRights CreateItems -User Anonymous

```

For detailed syntax and parameter information, see [Add-PublicFolderClientPermission](https://docs.microsoft.com/powershell/module/exchange/add-publicfolderclientpermission).
