---
localization_priority: Normal
description: You can remove a Microsoft Outlook on the web mailbox policy from an Exchange organization by using either the EAC or Exchange Online PowerShell.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: edab7bac-b62c-4b82-8f21-dcac77cf0e8f
ms.reviewer: 
title: Remove an Outlook on the web mailbox policy from Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Remove an Outlook on the web mailbox policy from Exchange Online

You can remove an Outlook on the web mailbox policy (formerly known as an Outlook Web App mailbox policy) from an Exchange Online organization by using either the Exchange admin center (EAC) or Exchange Online PowerShell.

**Note**: Don't remove the built-in mailbox policy named OwaMailboxPolicy-Default.

For additional management tasks related to Outlook on the web mailbox policies, see [Outlook on the web mailbox policies](outlook-web-app-mailbox-policies.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Outlook on the web mailbox policies" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to remove an Outlook on the web mailbox policy

1. In the EAC, go to **Permissions** \> **Outlook Web App policies**, select the policy that you want to remove, and then click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.png).

2. In the confirmation window that appears, click **Yes** to remove the mailbox policy, or click **No** to cancel.

## Use Exchange Online PowerShell to remove an Outlook on the web mailbox policy

To remove an Outlook on the web mailbox policy, use the following syntax:

```PowerShell
Remove-OwaMailboxPolicy -Identity "<Policy Name>"
```

This example removes the Outlook on the web mailbox policy named Sales Associates.

```PowerShell
Remove-OwaMailboxPolicy -Identity "Sales Associates"
```

For detailed syntax and parameter information, see [Remove-OwaMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/remove-owamailboxpolicy).

## How do you know this worked?

To verify that you've successfully removed an Outlook on the web mailbox policy, do any of the following steps:

- In the EAC, go to **Permissions** \> **Outlook Web App policies** and verify the policy is no longer listed.

- In Exchange Online PowerShell, run the following command to verify the policy is no longer listed:

    ```PowerShell
    Get-OwaMailboxPolicy
    ```
