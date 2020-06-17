---
localization_priority: Normal
description: Frequently asked questions about message trace.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: aa49e3f9-a5b1-4410-aac2-ddbbf3f5bfb2
ms.reviewer: 
f1.keywords:
- NOCSH
title: Message Trace FAQ
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Message Trace FAQ

This topic presents messaging questions that a user may have, along with possible answers. It also describes how to use the message trace tool in order to get those answers and troubleshoot specific mail delivery issues.

## How long does it take to see results when running a message trace?

- In the Exchange admin center (EAC), the search results appear immediately for messages that are less than 7 days old.

- In the Microsoft 365 Security Center, the search results appear immediately for messages that are less than 10 days old.

When you run a message trace for older messages, the results are returned within a few hours as a downloadable CSV file.

## How long does it take for a sent message to appear in a message trace?

When a message is sent, it should take between 5-10 minutes for the message to appear in the message trace data.

## Can I run a message trace via Exchange Online PowerShell or Exchange Online Protection PowerShell? What are the cmdlets to use?

You can use the following cmdlets in Exchange Online PowerShell or Exchange Online Protection PowerShell to run a message trace:

[Get-MessageTrace](https://docs.microsoft.com/powershell/module/exchange/get-messagetrace): Trace messages that are less than 10 days old.

[Get-MessageTraceDetail](https://docs.microsoft.com/powershell/module/exchange/get-messagetracedetail): View the message trace event details for a specific message.

[Get-HistoricalSearch](https://docs.microsoft.com/powershell/module/exchange/get-historicalsearch): Use this cmdlet to view information about historical searches that have been performed within the last 10 days.

[Start-HistoricalSearch](https://docs.microsoft.com/powershell/module/exchange/start-historicalsearch): Start a new historical search for messages that are less than 90 days old.

[Stop-HistoricalSearch](https://docs.microsoft.com/powershell/module/exchange/stop-historicalsearch): Stop queued historical searches that haven't started yet (the status value is `NotStarted`).

To connect to Exchange Online PowerShell, see [Connect to Exchange Online Using Remote PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

To connect to Exchange Online Protection PowerShell, see [Connect to Exchange Online Protection Using Remote PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-protection-powershell).

## Why am I getting a timeout error when running a message trace in the user interface?

The likely cause of a timeout error is that the query is taking too long to process. Consider simplifying your search criteria. You may want to consider using the **Get-MessageTrace** cmdlet, which has more liberal timeout requirements.

## Why didn't I receive an expected email message?

Here are some possible reasons:

- The message was detected as spam.

- The message was sent to quarantine due to a rule match.

- The message was rejected

  - By the malware filter

  - Because a file attached to the message contained malware

  - Because the message body contained malware

  - By a rule

  - Because the action was Reject

  - Because the action was Force TLS and TLS failed to be established

  - By a connector because TLS was required and failed to be established

- The message was sent for moderation and is awaiting approval or was rejected by the moderator.

- The message was never sent.

- The message is still being processed because there was a previous failure and the service is re-attempting delivery.

- The message failed to be delivered to your mailboxes

  - Because the destination is not reachable

  - Because the destination rejected the message

  - Because the message timed out during the delivery attempt

To find out what happened:

[Run a message trace](run-a-message-trace-and-view-results.md#run-a-message-trace). Use as many search criteria as possible to narrow down the results. For example, you should know the sender and the intended recipient or recipients of the message, and the general time period when the message was sent.

View the results, locate the message, and then view specific details about the message (see [View message trace results for messages less than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-less-than-7-days-old) or [View message trace results for messages more than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-more-than-7-days-old)). Look for a delivery status of **Failed** or **Pending** to explain why the message was not received.

Confirm that the message was sent, that it was successfully received by the service, that it was not filtered, redirected, or sent for moderation, and that it did not experience any delivery failures or delays.

## Why did I receive an unexpected message?

Here are some possible reasons:

- The message was released from quarantine.

- The message was awaiting moderator approval and was released.

- The message was spam that was not detected.

- The message matched a rule that added you to the message.

- The message was sent to a distribution list of which you are a member.

To find out what happened:

[Run a message trace](run-a-message-trace-and-view-results.md#run-a-message-trace). Use as many search criteria as possible to narrow down the results. For example, specify the recipient who received the message, set the delivery status to **Delivered**, and set the time period based on when the message was received.

View the results, locate the message, and then view specific details about the message (see [View message trace results for messages less than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-less-than-7-days-old) or [View message trace results for messages more than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-more-than-7-days-old)).


## Why didn't someone receive my message or why did I get this non-delivery report (also known as an NDR or bounce message)?

Possible reasons include the following:

- The message was detected as spam.

- The message was sent to quarantine due to a rule match.

- The message was re-routed because a connector sent it to another destination.

- The message was rejected

  - By the malware filter

  - Because a file attached to the message contained malware

  - Because the message body contained malware

  - By a rule

  - Because the action was Reject

  - Because the action was Force TLS and TLS failed to be established

  - By a connector because TLS was required and failed to be established

- The message was sent for moderation and is awaiting approval or was rejected by the moderator.

- The message was never sent.

- The message is still being processed because there was a previous failure and the service is re-attempting delivery.

- The message failed to be delivered to the destination

  - Because the destination is not reachable

  - Because the destination rejected the message

  - Because the message timed out during the delivery attempt

- The message was delivered to the destination but it was deleted before it was accessed (perhaps because it matched a rule).

To find out what happened:

[Run a message trace](run-a-message-trace-and-view-results.md). Use as many search criteria as possible to narrow down the results. For example, you should know the sender and the intended recipient or recipients of the message, and the general time period when the message was sent.

View the results, locate the message, and then view specific details about the message (see [View message trace results for messages less than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-less-than-7-days-old) or [View message trace results for messages more than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-more-than-7-days-old)).

Look for a delivery status of **Failed** or **Pending** to explain why the message was not delivered. Confirm that the message was sent, that it was successfully received by the service, that it was not filtered, redirected, or sent for moderation, and that it did not experience any delivery failures or delays. If the destination is not reachable, you can use the **To IP** to help troubleshoot connectivity issues.

## Why is my message taking so long to arrive to its destination? Where is it in the pipeline?

Possible reasons include the following:

- The intended destination is not responsive. This is the most likely scenario.

- It may be a large message that is taking a long time to process

- Latency in the service may be causing delays

- The message may have been blocked

To find out what happened:

[Run a message trace](run-a-message-trace-and-view-results.md#run-a-message-trace). Use as many search criteria as possible to narrow down the results. For example, you should know the sender and the intended recipient or recipients of the message, and the general time period when the message was sent.

View the results, locate the message, and then view specific details about the message (see [View message trace results for messages less than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-less-than-7-days-old) or [View message trace results for messages more than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-more-than-7-days-old)).

The events section will tell you why the message was not yet delivered. When viewing the events, the timestamp information will let you follow the message through the messaging pipeline, and tell you how long the service takes to process each event. The event details will also inform you if the message being delivered is extremely large or if the destination is not responsive.

## Was a message marked as spam?

Messages can be marked as spam for several reasons. For example, the sending IP address may appear on one of the service's IP Block lists. A message can be marked as spam due to the content of the actual message, such as when it matches a rule in the spam content filter. The message trace tool only tracks spam content filter events; connection filter events (such as blocked IP addresses) are not traceable. For more information about spam filtering, including spam content filtering, see [Anti-Spam Protection](https://docs.microsoft.com/office365/SecurityCompliance/anti-spam-and-anti-malware-protection).

To find out why a message was marked as spam:

[Run a message trace](run-a-message-trace-and-view-results.md#run-a-message-trace), locate the message in the results, and then view specific details about the message (see [View message trace results for messages less than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-less-than-7-days-old) or [View message trace results for messages more than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-more-than-7-days-old)).

When the content filter marks a message as spam, if it is sent to the Junk Email folder or the quarantine, it will have a status of **Delivered**. You can view the event details in order to see how the message arrived at its destination. For example, it may inform you that the message was determined to have a high spam confidence level, or that an advanced spam filtering option was matched. You will also be informed of the action that occurred as a result of the message being marked as spam, for example if it was sent to quarantine, stamped with an X-header, or if it was sent through the high risk delivery pool.

## Was a message detected to contain malware?

Messages are detected as malware when its properties, either in the message body or in an attachment, match a malware definition in of one of the anti-malware engines. For more detailed information about malware filtering, see [Anti-Malware protection](https://docs.microsoft.com/office365/SecurityCompliance/anti-malware-protection).

To find out why a message was detected to contain malware, [run a message trace](run-a-message-trace-and-view-results.md#run-a-message-trace). Use as many search criteria as possible to narrow down the results. Set the delivery status to **Failed**.

View the results, locate the message, and then view specific details about the message (see [View message trace results for messages less than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-less-than-7-days-old) or [View message trace results for messages more than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-more-than-7-days-old)).

If the message was not delivered because it was determined to contain malware, this information will be provided in the events section. For example, the following is a sample **Detail**: Malware: "*ZipBomb*" was detected in attachment *file*.*zip*. You will also be informed of the action that occurred as a result of the message containing malware, for example if the entire message was blocked or if all attachments were deleted and replaced with an alert text file.

## Which mail flow rule (also known as a transport rule) or DLP policy was applied to a message?

To find out which mail flow rule (custom policy rule) or data loss prevention (DLP) policy (Exchange Online customers only) was applied to a message, [run a message trace](run-a-message-trace-and-view-results.md#run-a-message-trace). Use as many search criteria as possible to narrow down the results. Set the delivery status to **Failed**.

View the results, locate the message, and then view specific details about the message (see [View message trace results for messages less than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-less-than-7-days-old) or [View message trace results for messages more than 7 days old](run-a-message-trace-and-view-results.md#view-message-trace-results-for-messages-more-than-7-days-old)).

If the message was not delivered because its contents matched a rule, the events section will let you know the name of the mail flow rule that was matched. You will also be informed of the action that occurred as a result of the mail flow rule match, for example if the message was quarantined, rejected, redirected, sent for moderation, decrypted, or any number of other possible options. For information about how to create Exchange mail flow rules and set actions for them, see [Mail flow rules (transport rules) in Exchange Online](../../security-and-compliance/mail-flow-rules/mail-flow-rules.md).

## When I run a message trace it returns rule ID-1. What does this mean?

Rule ID-1 is returned when the message trace encounters a mail flow rule that no longer exists. (The mail flow rule could have been modified or deleted after the original message was sent.)

## Are there any known limitations or behavior clarifications that I should be aware of when using the message trace tool?

You should be aware of the following when using the message trace tool:

- **IP-blocked messages**: Messages blocked by IP reputation block lists will be included in the spam data for real time reports, but you cannot perform a message trace on these messages.

- **Redirected messages**: If a recipient is rewritten by a mail flow rule or because the spam action for the domain is set to **Redirect to email address**, the message is not traceable in a single search. The original message can be traced until to the point when the recipient is changed. After that, the message is not traceable under the original recipient. You can trace the message again using the new recipient.

- **MAIL FROM**: The message trace tool uses the MAIL FROM value presented at the initiation of the SMTP conversation as the Sender in a search, regardless of what the DATA section of the message shows. The message may show a Reply-to address or different From: or Sender values. If the email message was sent by a process and not by an email client, there is an increased likelihood that the sender in the MAIL FROM will not match the sender in the actual message.

- **Mail flow rule updates**: When a message matches a mail flow rule, the rule ID is stored in the message trace and real time reporting databases. If you trace one of these messages, or drill down on rule details in a report, the message trace and real time reporting user interfaces dynamically pull the current rule information from the hosted services network based on the rule ID in the reporting database. If you have changed the attributes of that particular rule since the message was processed (changed it from Reject to Allow, for example), the rule ID stays the same in the message trace and real time reporting returned results, but the Exchange admin center will show the new mail flow rule properties. You can use the auditing reports feature in order to determine when the rule was changed and the properties that were changed.

- **Spam-filtered messages**: When the content filter marks a message as spam, if it is sent to the Junk Email folder or the quarantine, it will have a status of **Delivered**. Drill down to the event details in order to see how the message arrived at its destination.

## For more information

[Trace an email message](trace-an-email-message.md)
