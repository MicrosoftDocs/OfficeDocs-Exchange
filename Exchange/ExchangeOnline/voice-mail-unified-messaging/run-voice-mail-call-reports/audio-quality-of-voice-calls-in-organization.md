---
localization_priority: Normal
description: If your organization is experiencing problems with the audio quality of Unified Messaging (UM) calls and voice mail messages, use the Call Statistics report to help you understand what's causing the problems.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 8a87694b-1678-4a01-859f-5ad3b2c73db5
ms.reviewer: 
f1.keywords:
- NOCSH
title: Investigate the audio quality of voice calls in your organization in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Investigate the audio quality of voice calls in your organization in Exchange Online

If your organization is experiencing problems with the audio quality of Unified Messaging (UM) calls and voice mail messages, use the Call Statistics report to help you understand what's causing the problems.

> [!NOTE]
> The audio quality of a call can be affected by factors that aren't covered in the reports. For example, if your Exchange servers are experiencing a heavy memory load or CPU load, users may report poor call quality, even though the reports show excellent audio quality.

For additional tasks related to call statistics see [UM reports procedures](um-reports-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to get audio quality statistics for your organization

1. In the EAC, navigate to **Unified messaging** \> **More options** ![More Options Icon](../../media/ITPro_EAC_MoreOptionsIcon.gif) \> **Call statistics**.

2. Choose the call statistics to include in the report. The report automatically updates as you select any of the following options.

   - **Show**: Choose what type of call statistics to view:

   - **Daily (90 days)**: Select Daily to see details for all calls in the past 90 days.

   - **Monthly (12 months)**: Select Monthly to see a summary of calls by month for the last 12 months.

   - **All**: Select All to see the combined statistics for all calls received since UM started handling calls.

   - **UM dial plan**: If you want to limit the data in the report to only calls in a specific UM dial plan, select that dial plan.

   - **UM IP gateway**: If you want to limit the data in the report to only calls in a specific UM IP gateway, select that UM IP gateway. If you select a UM dial plan first, only the UM IP gateways associated with the selected UM dial plan are available in the list.

3. To get more details about the audio quality for a row in the report, select the row and click **Audio Quality Details**. The following information is available:

   - **DATE AND TIME**: The UTC date and time that the call statistics were captured.

   - **UM DIAL PLAN**: The dial plan for the calls included in the statistics.

   - **UM IP GATEWAY**: The UM IP gateway that took the calls included in the statistics.

   - **NMOS**: The Network Mean Opinion Score (NMOS) for the call. The NMOS indicates how good the audio quality was on the call as a number on a scale from 1 to 5, with 5 being excellent.

     > [!NOTE]
     > The maximum NMOS possible for a call is dependent on the audio codec being used. The NMOS may not be available for very short calls that are less than 10 seconds long.

   - **NMOS DEGRADATION**: The amount of audio degradation of the NMOS from the top value possible for the audio codec being used. For example, if the NMOS degradation value for a call was 1.2 and the NMOS reported for the call was 3.3, the maximum NMOS for that particular call would be 4.5 (1.2 + 3.3).

   - **JITTER**: The average variation in the arrival of data packets for the call.

   - **PACKET LOSS**: The average percentage of data packet loss for the selected call. Packet loss is an indication of the reliability of the connection.

   - **ROUND TRIP**: The average round trip score, in milliseconds, for audio on the selected call. The round-trip score measures latency on the connection.

   - **BURST LOSS DURATION**: The average duration of packet loss during bursts of losses for the selected call.

   - **NUMBER OF SAMPLES**: The number of calls that were sampled to calculate the averages.

4. For detailed audio quality metrics for specific calls, see [Investigate the audio quality of voice calls for a user](audio-quality-of-voice-calls-for-user.md).
