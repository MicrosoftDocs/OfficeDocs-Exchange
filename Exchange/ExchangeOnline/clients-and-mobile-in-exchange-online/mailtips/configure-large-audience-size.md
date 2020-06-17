---
localization_priority: Normal
description: You can use Exchange Online PowerShell to configure various settings that define how you use MailTips in your organization.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 8a37911c-4339-4921-b5d3-0a5a774d4517
ms.reviewer: 
title: Configure the large audience size for your organization
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Configure the large audience size for your organization

You can use Exchange Online PowerShell to configure various settings that define how you use MailTips in your organization.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "MailTips" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- You can only use Exchange Online PowerShell to perform this procedure.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to configure the large audience size for your organization

You use the **Set-OrganizationConfig** cmdlet to configure the large audience size for your organization. When the sender adds a distribution group that has more members than the configured large audience size, they are shown the Large Audience MailTip. The large audience size is set to 25 by default. This example configures the large audience size to 50 in your organization.

```Set-OrganizationConfig
Set-OrganizationConfig -MailTipsLargeAudienceThreshold 50
```

For detailed syntax and parameter information, see [set-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/set-organizationconfig).
