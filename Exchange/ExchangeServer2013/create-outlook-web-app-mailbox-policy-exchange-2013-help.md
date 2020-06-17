---
title: 'Create an Outlook Web App mailbox policy: Exchange 2013 Help'
TOCTitle: Create an Outlook Web App mailbox policy
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 347207fa-cfb7-40a6-b19a-831dcdb54ad5
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create an Outlook Web App mailbox policy in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can create an Outlook Web App mailbox policy to apply a common set of policy settings. Outlook Web App mailbox policies are useful for applying and standardizing settings, for example, attachment settings, for specific groups of users.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can run this cmdlet. Although all parameters for this cmdlet are listed in this topic, you may not have access to some parameters if they're not included in the permissions assigned to you. To see what permissions you need, see the "Outlook Web App mailbox policies" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to create an Outlook Web App mailbox policy

1. In the EAC, click **Permissions** \> **Outlook Web App policies**.

2. Click the **New** button.

3. Enter a name for your policy.

4. Use the check boxes to enable or disable features. By default, the most common features are displayed. To see all features that can be enabled or disabled, click **More options**.

    > [!NOTE]
    > Features settings for Outlook Web App mailbox policies override Outlook Web App virtual directory settings. You can change segmentation settings for individual users by using the **Set-CASMailbox** cmdlet in the Shell.

5. Click **Save** to save the policy.

## Use the Shell to create an Outlook Web App mailbox policy

This example creates an Outlook Web App mailbox policy named `Policy1`.

- In the Shell, run the following command.

  ```powershell
  New-OwaMailboxPolicy -Name Policy1
  ```

For more information about syntax and parameters, see [New-OwaMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/new-owamailboxpolicy). For information about using the Shell to configure an Outlook Web App mailbox policy, see [Set-OwaMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/set-owamailboxpolicy).

## How do you know this worked?

To verify that you've successfully created an Outlook Web App mailbox policy:

- In the EAC, click **Permissions** \> **Outlook Web App Policies**, and look for your new mailbox policy.
