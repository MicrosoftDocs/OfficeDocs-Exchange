---
title: 'Set the default language on a dial plan: Exchange 2013 Help'
TOCTitle: Set the default language on a dial plan
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 7a1d2e7e-4053-40af-9ec1-ec714df12ad4
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set the default language on a dial plan in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can set the default language for a Unified Messaging (UM) dial plan. Each dial plan you create will initially use English (en-US) as the default language. The English (en-US) language pack is installed on all versions of Microsoft Exchange Server 2013 and can't be removed.

If you want to select another language as the default language, for example, German (de-DE), you must first download the German UM language pack .exe file from [Exchange Server 2013 UM Language Packs](https://www.microsoft.com/download/details.aspx?id=35368) and install that language pack on the Mailbox server by using the UMLanguagePack.de-de.exe installation file. After you've installed the language pack, you can set the default language to a language other than English (en-US) on your UM dial plans.

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to set the default language on a UM dial plan

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan that you want to modify, and then, on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. On the **Settings** page, under **Audio language**, select the language you want to set from the drop-down list.

5. Click **Save** to accept your changes.

## Use the Shell to set the default language on a UM dial plan

This example sets the default language on a UM dial plan named `MyUMDialPlan` to German.

```powershell
Set-UMDialPlan -Identity MyUMDialPlan -DefaultLanguage de-DE
```

This example sets the default language on a UM dial plan named `MyUMDialPlan` to Japanese.

```powershell
Set-UMDialPlan -Identity MyUMDialPlan -DefaultLanguage ja-JP
```

This example sets the default language on a UM dial plan named `MyUMDialPlan` to Australian English.

```powershell
Set-UMDialPlan -Identity MyUMDialPlan -DefaultLanguage en-AU
```
