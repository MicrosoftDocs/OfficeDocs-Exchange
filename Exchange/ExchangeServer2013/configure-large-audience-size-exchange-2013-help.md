---
title: 'Configure the large audience size for your organization: Exchange 2013 Help'
TOCTitle: Configure the large audience size for your organization
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 8a37911c-4339-4921-b5d3-0a5a774d4517
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the large audience size for your organization in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use the Exchange Management Shell to configure various settings that define how you use MailTips in your organization.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "MailTips" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to configure the large audience size for your organization

You use the **Set-OrganizationConfig** cmdlet to configure the large audience size for your organization. When senders address messages to more recipients than the size you configure, they are shown the Large Audience MailTip. The large audience size is set to 25 by default. This example configures the large audience size to 50 in your organization.

```powershell
Set-OrganizationConfig -MailTipsLargeAudienceThreshold 50
```

For detailed syntax and parameter information, see [Set-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/set-organizationconfig).
