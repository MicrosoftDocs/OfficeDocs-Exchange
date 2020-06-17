---
title: 'Import custom prompts from Exchange 2007 to Exchange 2013: Exchange 2013 Help'
TOCTitle: Import custom prompts from Exchange 2007 to Exchange 2013
ms:assetid: 70c0b0bc-c0de-4e3c-8144-1fe59f86ebf4
ms:mtpsurl: https://technet.microsoft.com/library/Gg309147(v=EXCHG.150)
ms:contentKeyID: 53382779
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Import custom prompts from Exchange 2007 to Exchange 2013

_**Applies to:** Exchange Server 2013_

You can import the audio files that contain custom greetings, announcements, menus, and prompts from Exchange 2007 Unified Messaging (UM) to Exchange 2013 Unified Messaging. Using a Shell script, the prompts are imported into an Exchange system mailbox named {e0dc1c29-89c3-4034-b678-e6c29d823ed9}, which is created when you install Microsoft Exchange 2013. This system mailbox is used in Unified Messaging to store dial plan and auto attendant custom greetings, announcements, menus, prompts, and UM reports.

The audio files, in .wav or .wma format, are used as follows:

- On UM dial plans, audio files are used for customized welcome greetings and informational announcements. They're played when Outlook Voice Access users call in to an Outlook Voice Access number.

- On UM auto attendants, audio files are used for customized non-business and business hours greetings, informational announcements, menu prompts, and navigation menus. They're played when callers call in to a UM auto attendant.

You use the MigrateUMCustomPrompts.ps1 script to migrate a copy of all Exchange Server 2007 UM custom greetings, announcements, menus, and prompts to Exchange 2013 UM for all Exchange 2007 UM dial plans and UM auto attendants.

By default, the MigrateUMCustomPrompts.ps1 script is located in the \<Program Files\>\\Microsoft\\Exchange Server\\V15\\Scripts folder on an Exchange 2013 server.

> [!NOTE]
> The MigrateUMCustomPrompts.ps1 script is included with Exchange 2013. It must be run on an Exchange 2013 Mailbox server in the same organization with your Exchange 2007 UM servers.

For additional management tasks related to UM dial plans, see [UM dial plan procedures](um-dial-plan-procedures-exchange-2013-help.md).

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/um-auto-attendant-procedures).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" and "UM auto attendants" entries in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/create-a-um-auto-attendant).

- You can only use the Shell to perform this procedure. To learn how to open the Shell in your on-premises Exchange organization, see [Open the Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the MigrateUMCustomPrompts.ps1 script to migrate a copy of all custom prompts for UM dial plans and auto attendants

1. Click **Start** \> **All Programs** \> **Microsoft Exchange Server 2013** \> **Exchange Management Shell**.

2. In the Shell, at the prompt, type the path to the script. For example, type **cd "D:\\Program Files\\Microsoft\\Exchange Server\\V15\\Scripts"**, and then press Enter.

3. At the Shell prompt, type **".\\MigrateUMCustomPrompt",** and then press Enter.
