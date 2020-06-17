---
localization_priority: Normal
description: 'Unified Messaging can use one of four codecs for creating voice mail messages: MP3, Windows Media Audio (WMA), Group System Mobile (GSM) 06.10, and G.711 Pulse Code Modulation (PCM) Linear. By default, when you create a Unified Messaging (UM) dial plan, the UM dial plan uses the MP3 audio codec to record voice messages. The MP3 audio format is a popular audio format that is used across multiple operating systems, email clients, and MP3 players. After the UM dial plan is created, you can configure the UM dial plan to use one of the other audio formats including the WMA, GSM 06.10, or G.711 PCM Linear audio codecs. To listen to the voice message, a mobile phone or computer must have a compatible audio software application installed.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 139b2ccd-28c5-46c0-9050-777f4f59aade
ms.reviewer: 
f1.keywords:
- NOCSH
title: Change the audio codec in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Change the audio codec in Exchange Online

Unified Messaging can use one of four codecs for creating voice mail messages: MP3, Windows Media Audio (WMA), Group System Mobile (GSM) 06.10, and G.711 Pulse Code Modulation (PCM) Linear. By default, when you create a Unified Messaging (UM) dial plan, the UM dial plan uses the MP3 audio codec to record voice messages. The MP3 audio format is a popular audio format that is used across multiple operating systems, email clients, and MP3 players. After the UM dial plan is created, you can configure the UM dial plan to use one of the other audio formats including the WMA, GSM 06.10, or G.711 PCM Linear audio codecs. To listen to the voice message, a mobile phone or computer must have a compatible audio software application installed.

For additional tasks related to UM dial plans, see [UM dial plan procedures in Exchange Online](um-dial-plan-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to change the audio codec on a Unified Messaging dial plan

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. In **Settings**, under **Audio codec**, use the drop-down list to select one the following:

   - MP3

   - WMA

   - GSM

   - G711

5. Click **Save**.

## Use Exchange Online PowerShell to change the audio codec on a Unified Messaging dial plan

This example sets the audio codec on a UM dial plan named `MyUMDialPlan` to G.711.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -AudioCodec G711
```

This example sets the audio codec on a UM dial plan named `MyUMDialPlan` to WMA.

```PowerShell
Set-UMDialPlan -Identity MyUMDialPlan -AudioCodec Wma
```
