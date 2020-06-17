---
localization_priority: Normal
description: Admins can learn how to view and modify Outlook on the web mailbox policies (formerly known as Outlook Web App mailbox policies) in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: be012ffe-8fdb-4fb7-aebd-78b3a55593fa
ms.reviewer: 
title: View or configure Outlook on the web mailbox policy properties in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# View or configure Outlook on the web mailbox policy properties in Exchange Online

After you create an Outlook on the web mailbox policy, you can configure a variety of options to control the features available to users in Outlook on the web (formerly known as Outlook Web App). For example, you can enable or disable Inbox rules or create a list of allowed file types for attachments.

For more information about Outlook on the web mailbox policies, see [Outlook on the web mailbox policies](outlook-web-app-mailbox-policies.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Outlook on the web mailbox policies" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to view or configure Outlook on the web mailbox policies

1. In the EAC, go to **Permissions** \> **Outlook Web App policies** and select the policy that you want to view or configure.

2. The Details pane show the enabled features in the policy. To see more information, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png). In the properties window that opens you can view and configure the following settings:

   - On the **General** tab, you can view and edit the name of the policy.

   - On the **Features** tab, use the check boxes to enable or disable features. By default, the most common features are displayed. To see all features that can be enabled or disabled, click **More options**.

   **Note**: You can configure settings for individual users by using the **Set-CASMailbox** cmdlet in Exchange Online PowerShell.

   - On the **File Access** tab, use the **Direct file access** check boxes to configure the file access and viewing options for users. File access lets a user open or view the contents of files attached to an email message.

    File access can be controlled based on whether a user has signed in on a public or private computer. The option for users to select private computer access or public computer access is available only when you're using forms-based authentication. All other forms of authentication default to private computer access.

   - On the **Offline access** tab, use the option buttons to configure offline access availability.

3. When you're finished, click **Save** to update the policy.

## Use Exchange Online PowerShell to modify Outlook on the web mailbox policies

To modify an Outlook on the web mailbox policy, use the following syntax:

```PowerShell
Set-OwaMailboxPolicy -Identity "<Policy Name>" [Settings]
```

This example enables calendar access in the default mailbox policy.

```PowerShell
Set-OwaMailboxPolicy -Identity Default -CalendarEnabled $true
```

For detailed syntax and parameter information, see [Set-OwaMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/set-owamailboxpolicy).

## Use Exchange Online PowerShell to view Outlook on the web mailbox policies

To view an Outlook on the web mailbox policy, use the following syntax:

```PowerShell
Get-OwaMailboxPolicy [-Identity "<Policy Name>"]
```

This example returns a summary list of all policies in the organization

```PowerShell
Get-OwaMailboxPolicy | Format-Table Name
```

This example retrieves detailed information for the policy named Executives.

```PowerShell
Get-OwaMailboxPolicy -Identity Executives
```

For detailed syntax and parameter information, see [Get-OwaMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/get-owamailboxpolicy).

## How do you know this worked?

To verify that you've successfully modified an Outlook on the web mailbox policy, do either of the following steps:

- In the EAC, click **Permissions** \> **Outlook Web App Policies**, select the policy, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png), and verify the properties of the policy.

- In Exchange Online PowerShell, replace \<Policy Name\> with the name of the policy, and run the following command to verify the settings:

  ```PowerShell
  Get-OwaMailboxPolicy -Identity "<Policy Name>"
  ```
