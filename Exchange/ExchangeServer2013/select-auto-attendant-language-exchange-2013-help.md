---
title: 'Select the language for an auto attendant: Exchange 2013 Help'
TOCTitle: Select the language for an auto attendant
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 3a1c1ec0-c726-41fb-a294-59faab205609
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Select the language for an auto attendant in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can configure the default prompt language setting on a Unified Messaging (UM) auto attendant. The language setting available on a UM auto attendant enables you to configure the default prompt language on the auto attendant. When you're using the default system prompts for the auto attendant, this is the language that the caller hears when the auto attendant answers the incoming call. This language setting affects only the default system prompts that are provided after you have installed the Mailbox server that is running the Microsoft Exchange Unified Messaging service. This setting doesn't affect custom prompts that are configured on an auto attendant. The languages that are available are based on the Unified Messaging language packs that are installed on the Mailbox server.

Each auto attendant you create will initially use English (en-US) as the default language. The English (en-US) language pack is installed by default on all versions of Microsoft Exchange 2013 and can't be removed. If you want to select another language, for example, German (de-DE), you must first download the German UM language pack .exe file from [Exchange Server 2013 UM Language Packs](https://www.microsoft.com/download/details.aspx?id=35368) and install the UM language pack on the Mailbox server by using the UMLanguagePack.de-de.exe installation file. After you've installed the UM language pack, you can set the default language to a language other than English (en-US) on UM auto attendants.

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure the default language setting

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to modify, and then on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

4. On the **General** page, under **Language for automated voice interface**, select the required language from the drop-down list.

5. Click **Save** to accept your changes.

## Use the Shell to configure the default language setting

This example sets the default language on the UM auto attendant `MyUMAutoAttendant` to English (Great Britain).

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -Language en-GB
```

This example sets the default language on the UM auto attendant `MyUMAutoAttendant` to German.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -Language de-DE
```
