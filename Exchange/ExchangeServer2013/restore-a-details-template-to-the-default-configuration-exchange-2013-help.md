---
title: 'Restore a details template to the default configuration: Exchange 2013 Help'
TOCTitle: Restore a details template to the default configuration
ms:assetid: 84c5f49b-614d-4f0e-8701-0979a2eb90bf
ms:mtpsurl: https://technet.microsoft.com/library/Bb232102(v=EXCHG.150)
ms:contentKeyID: 49289330
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Restore a details template to the default configuration

_**Applies to:** Exchange Server 2013_

The Details Templates Editor doesn't contain an **Undo** button, nor can you use a keyboard shortcut to undo an action. To undo an addition you made to the template, you must use the DELETE key. To undo a deletion, you must reapply the setting. You can also revert to the original settings by exiting the Details Templates Editor without saving your changes. If you want to undo changes after you have saved, you can restore the template. When you restore a template, all customization is lost, and the template is restored to its original configuration.

This topic explains how to use the Exchange 2013 Toolbox or the Exchange Management Shell to restore a details template to its default configuration.

To learn more about details templates, see [Details templates](details-templates-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Details templates" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Exchange Toolbox to restore a details template to its default configuration

1. Click **Start** \> **All Programs** \> **Microsoft Exchange Server 2013** \> **Exchange Toolbox**.

2. In **Exchange Toolbox**, click **Details Templates Editor**, and then, in the action pane, click **Open Tool**.

3. In the **Details Templates Editor**, in the details pane, select the template you want to restore, and then in the action pane, click **Restore**.

4. Click **Yes** to confirm that you want to restore the template to its original state. All customization will be lost.

## Use the Shell to restore a details template to its default configuration

This example restores the United States English contacts details template.

```powershell
Restore-DetailsTemplate -Identity "en-US\Contact"
```

For detailed syntax and parameter information, see [Restore-DetailsTemplate](https://docs.microsoft.com/powershell/module/exchange/Restore-DetailsTemplate).
