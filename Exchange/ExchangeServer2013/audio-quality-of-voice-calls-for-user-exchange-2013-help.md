---
title: 'Investigate the audio quality of voice calls for a user: Exchange 2013 Help'
TOCTitle: Investigate the audio quality of voice calls for a user
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 0c945886-3cfa-423e-9b46-0d6b1584a145
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Investigate the audio quality of voice calls for a user in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

If a user reports problems with the audio quality of their Unified Messaging (UM) calls, you can use the User Call Logs report to help you understand what's causing the problems.

> [!NOTE]
> The audio quality of a call can be affected by factors that aren't covered in the reports. For example, if your Exchange servers are experiencing a heavy memory or CPU load, users may report poor call quality, even though the reports show excellent audio quality.

For additional tasks related to UM reports, see [UM reports procedures](um-reports-procedures-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM call data and summary report cmdlets" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to get call logs for a UM-enabled user

1. In the EAC, navigate to **Unified Messaging** \> **More options** ![More Options Icon](images/ITPro_EAC_MoreOptionsIcon.gif) \> **User call logs**.

2. Click **Select a user**, and then select the user you want data for.

3. To get more details about the audio quality for a row in the report, select the row and click **Audio Quality Details**. The following information is available:

   - **DATE AND TIME**: The date and time of the call, in the time zone that the selected user has set in Outlook Web App.

   - **USER**: The selected user.

   - **UM DIAL PLAN**: The dial plan for the call.

   - **UM IP GATEWAY**: The UM IP gateway that was used for the call.

   - **AUDIO CODEC**: The audio codec that was used during the call.

   - **NMOS**: The Network Mean Opinion Score (NMOS) for the call. The NMOS indicates how good the audio quality was on the call as a number on a scale from 1 to 5, with 5 being excellent.

     > [!NOTE]
     > The maximum NMOS possible for a call depends on the audio codec being used. The NMOS may not be available for very short calls that are less than 10 seconds long.

   - **NMOS DEGRADATION**: The amount of audio degradation of the NMOS from the top value possible for the audio codec being used. For example, if the NMOS degradation value for a call was 1.2 and the NMOS reported for the call was 3.3, the maximum NMOS for that particular call would be 4.5 (1.2 + 3.3).

   - **JITTER**: The average variation in the arrival of data packets for the call.

   - **PACKET LOSS**: The average percentage of data packet loss for the selected call. Packet loss is an indication of the reliability of the connection.

   - **ROUND TRIP**: The average round trip score, in milliseconds, for audio on the selected call. The round-trip score measures latency on the connection.

   - **BURST LOSS DURATION**: The average duration of packet loss during bursts of losses for the selected call.
