---
localization_priority: Normal
description: You can create an Outlook on the web mailbox policy to apply a common set of policy settings. Outlook on the web mailbox policies are useful for applying and standardizing settings, for example, attachment settings, for specific groups of users.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 347207fa-cfb7-40a6-b19a-831dcdb54ad5
ms.reviewer: 
title: Create an Outlook on the web mailbox policy in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Create an Outlook on the web mailbox policy in Exchange Online

You can create Outlook on the web mailbox policies to apply settings to users in Outlook on the web (formerly known as Outlook Web App). Outlook on the web mailbox policies are useful for applying and standardizing settings, for example, attachment settings, for specific groups of users.

For more information about Outlook on the web mailbox policies, see [Outlook on the web mailbox policies](outlook-web-app-mailbox-policies.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Outlook on the web mailbox policies" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create an Outlook on the web mailbox policy

1. In the EAC, go to **Permissions** \> **Outlook Web App policies**, and click **New** ![New icon](../../media/ITPro_EAC_AddIcon.png)

2. In the new policy window that opens, configure the following settings:

   - **Policy name**: Enter a unique name for your policy.

   - Use the check boxes to enable or disable features. By default, the most common features are displayed. To see all features that can be enabled or disabled, click **More options**.

   **Note**: You can configure settings for individual users by using the **Set-CASMailbox** cmdlet in Exchange Online PowerShell.

3. Click **Save** to save the policy.

## Use Exchange Online PowerShell to create an Outlook on the web mailbox policy

In Exchange Online PowerShell, creating an Outlook on the web mailbox policy is a two-step process:

1. Create the policy by using the following syntax:

   ```PowerShell
   New-OwaMailboxPolicy -Name "<Unique Name>"
   ```

   This example creates an Outlook on the web mailbox policy named Executives.

   ```PowerShell
   New-OwaMailboxPolicy -Name Policy1
   ```

    For detailed syntax and parameter information, see [New-OwaMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/new-owamailboxpolicy).

2. Modify the default settings of the policy.

   For more information, see [Use Exchange Online PowerShell to modify Outlook on the web mailbox policies](configure-outlook-web-app-mailbox-policy-properties.md#use-exchange-online-powershell-to-modify-outlook-on-the-web-mailbox-policies).

## How do you know this worked?

To verify that you've successfully created an Outlook on the web mailbox policy:

- In the EAC, click **Permissions** \> **Outlook Web App Policies**, and look for your new mailbox policy.

To verify that you've successfully created an Outlook on the web mailbox policy, do either of the following steps:

- In the EAC, click **Permissions** \> **Outlook Web App Policies**, and verify the policy is listed. You can select the policy and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) to verify the properties of the policy.

- In Exchange Online PowerShell, run the following command to verify the policy is listed:

  ```PowerShell
  Get-OwaMailboxPolicy | Format-Table Name
  ```

- In Exchange Online PowerShell, replace \<Policy Name\> with the name of the policy, and run the following command to verify the settings:

  ```PowerShell
  Get-OwaMailboxPolicy -Identity "<Policy Name>"
  ```

## Next steps

To modify an existing Outlook on the web mailbox policy, see [View or configure Outlook on the web mailbox policy properties in Exchange Online](configure-outlook-web-app-mailbox-policy-properties.md).
