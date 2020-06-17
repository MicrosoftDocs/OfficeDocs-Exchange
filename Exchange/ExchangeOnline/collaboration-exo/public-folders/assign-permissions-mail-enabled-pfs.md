---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.service: exchange-online
description: 'Summary: How to assign "Send As" or "Send on Behalf" permissions to your Exchange Online public folders.'
audience: ITPro
f1.keywords:
- NOCSH
title: Assign "Send As" or "Send on Behalf" permissions for mail-enabled public folders

---

# Assign "Send As" or "Send on Behalf" permissions for mail-enabled public folders

You can assign either "Send As" or "Send on Behalf" permissions for mail-enabled public folders to users in Microsoft Exchange Online.

## What do you need to know before you begin?

- Estimated time to complete this task: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in [Sharing and collaboration permissions](https://docs.microsoft.com/Exchange/permissions/feature-permissions/sharing-and-collaboration-permissions?view=exchserver-2019).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](https://docs.microsoft.com/exchange/accessibility/keyboard-shortcuts-in-admin-center).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [**Exchange Online**](https://go.microsoft.com/fwlink/p/?linkId=267542) or [**Exchange Online Protection**](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange admin center (EAC) to assign permissions

1. Sign in to Exchange admin center as an administrator.

2. Select **public folders** \> **public folders**.

3. In the list view, select the public folder that requires the permissions, and then click **Edit** (the pencil icon).

4. Select **delivery options**, and then add the user to **Send As** or **Send on Behalf** permissions, as required.

5. Select **Save**.

## Use Exchange Online PowerShell to assign permissions

The following example assigns "Send on Behalf" permissions for the mail-enabled public folder *NewPF1* to the user *Jason*.

`Set-MailPublicFolder -Identity '\\NewPF1' -GrantSendOnBehalfTo "Jason"`

The following example assigns "Send As" permissions for the mail-enabled public folder *NewPF1* to the user *Jason*.

`Add-RecipientPermission -Identity 'NewPF1' -Trustee "Jason" -AccessRights 'SendAs'`

For detailed syntax and parameter information, see the following articles:

- [Set-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/set-mailpublicfolder?view=exchange-ps)

- [Add-RecipientPermission](https://docs.microsoft.com/powershell/module/exchange/add-recipientpermission?view=exchange-ps)
