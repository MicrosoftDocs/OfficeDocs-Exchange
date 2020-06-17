---
title: 'Content filtering: Exchange 2013 Help'
TOCTitle: Content filtering
ms:assetid: d660ffbf-de05-46c2-940b-5200eca94e0a
ms:mtpsurl: https://technet.microsoft.com/library/Bb124739(v=EXCHG.150)
ms:contentKeyID: 49248690
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Content filtering

_**Applies to:** Exchange Server 2013_

> [!NOTE]
> On November 1, 2016, Microsoft stopped producing spam definition updates for the SmartScreen filters in Exchange and Outlook. The existing SmartScreen spam definitions will be left in place, but their effectiveness will likely degrade over time. For more information, see <A href="https://go.microsoft.com/fwlink/p/?linkid=835894">Deprecating support for SmartScreen in Outlook and Exchange</A>.

The Content Filter agent evaluates inbound email messages and assesses the probability that an inbound message is legitimate or spam. Unlike many other filtering technologies, the Content Filter agent uses characteristics from a statistically significant sample of email messages. The inclusion of legitimate messages in this sample reduces the chance of mistakes. Because the Content Filter agent recognizes characteristics of legitimate messages and spam, its accuracy is increased. Updates to the Content Filter agent are available periodically through Microsoft Update.

## Using the Content Filter agent

The Content Filter agent is one of several anti-spam agents in Exchange. When you configure anti-spam agents on an Exchange server, the agents act on messages cumulatively to reduce the amount of spam that enters the organization. For more information about how to plan and deploy anti-spam agents, see [Anti-spam protection](anti-spam-protection-exchange-2013-help.md).

The Content Filter agent assigns a spam confidence level (SCL) rating to each message. The SCL rating is a number between 0 and 9. A higher SCL rating indicates that a message is more likely to be spam.

You can configure the Content Filter agent to take the following actions on messages according to their SCL rating:

- Delete message

- Reject message

- Quarantine message

For example, you may determine that messages that have an SCL rating of 7 or higher must be deleted, messages that have an SCL rating of 6 must be rejected, and messages that have an SCL rating of 5 must be quarantined.

You can adjust the SCL threshold behavior by assigning different SCL ratings to each of these actions. For more information about how to adjust the SCL threshold to suit your organization's requirements and about per-recipient SCL thresholds, see [Spam Confidence Level Threshold](spam-confidence-level-threshold-exchange-2013-help.md).

> [!NOTE]
> Messages that are over 11&nbsp;MB aren't scanned by the Intelligent Message Filter. Instead, they pass through the Content Filter without being scanned.

## Allow phrases and Block phrases

You can customize how the Content Filter agent assigns SCL values by configuring custom words. Custom words are individual words or phrases that the Content Filter agent uses to apply appropriate filter processing. You configure approved words or phrases with Allow phrases and unapproved words or phrases with Block phrases. When the Content Filter agent detects a preconfigured Allow phrase in an inbound message, the Content Filter agent automatically assigns an SCL value of 0 to the message. Alternatively, when the Content Filter agent detects a configured Block phrase in an inbound message, the Content Filter agent assigns an SCL rating of 9.

You can enter custom words or phrases in any combination of uppercase and lowercase letters. However, when the Content Filter agent evaluates message content, it ignores case. The maximum number of custom words or phrases that can be created is 800.

## Outlook Email Postmark validation

The Content Filter agent also includes Microsoft Outlook Email Postmark validation, a computational proof that Outlook applies to outgoing messages to help recipient messaging systems distinguish legitimate email from junk email. This feature helps reduce the chance of false positives. In the context of spam filtering, a *false positive* exists when a spam filter incorrectly identifies a message from a legitimate sender as spam. When Outlook Email Postmark validation is enabled, the Content Filter agent parses the inbound message for a computational postmark header. The presence of a valid, solved computational postmark header in the message indicates that the client computer that generated the message solved the computational postmark.

Computers don't require significant processing time to solve individual computational postmarks. However, processing postmarks for many messages may be prohibitive to a malicious sender. Anyone who sends millions of spam messages is unlikely to invest the processing power that is required to solve computational postmarks for all outbound spam. If a sender's email contains a valid, solved computational postmark, it's unlikely that the sender is a malicious sender. In this case, the Content Filter agent would lower the SCL rating. If the postmark validation feature is enabled and an inbound message either doesn't contain a computational postmark header or the computational postmark header isn't valid, the Content Filter agent would not change the SCL rating.

## Bypassing the recipient, sender, and sender domain

In some organizations, all email to certain aliases must be accepted. This scenario can introduce problems if your organization is in an industry that manages significant volumes of spam.

For example, a company named Woodgrove Bank has an alias named customerloans@woodgrovebank.com that provides email-based support to external loan customers. The Exchange administrators configure the Content Filter agent to set Block phrases that filter out words or phrases that are typically used in spam that is sent by unscrupulous loan agencies. To prevent potentially legitimate messages from being rejected, the administrators set exceptions to content filtering by entering a list of SMTP email recipient addresses in the Content Filter agent configuration.

You can also specify senders and sender domains that you do not want the Content Filter agent to block.

## Safelist aggregation

In Exchange 2013, the Content Filter agent uses the Outlook Safe Senders Lists, Blocked Sender List, Safe Recipients Lists, and trusted contacts from Outlook to optimize spam filtering. *Safelist aggregation* is a set of anti-spam functionality that is shared across Outlook and Exchange. As its name suggests, this functionality collects data from the anti-spam safe lists that Outlook users configure and makes this data available to the anti-spam agents on the Exchange server. Email messages that Outlook users receive from contacts that those users have added to their Outlook Safe Recipients List, Safe Senders List, or trusted contacts list are identified by the Content Filter agent as safe. The Sender Filter agent also performs per-recipient sender filtering using the Blocked Senders list that users configure. For more information, see [Safelist Aggregation](safelist-aggregation-exchange-2013-help.md).

## Configuring the Content Filter agent

You configure the Content Filter agent by using the Exchange Management Shell.

> [!IMPORTANT]
> Configuration changes that you make to the Content Filter agent in the Exchange Management Shell are only made on the local computer. If you have the Content Filter agent running on multiple Exchange servers in your organization, you must make Content Filter configuration changes to each computer.

The Content Filter agent depends on updates to determine whether a message can be delivered with confidence that it isn't spam. These updates contain data about phishing Web sites, Microsoft SmartScreen spam heuristics, and other Intelligent Message Filter updates. Content filter updates generally contain about 6 MB of data that's useful for longer periods of time than other anti-spam update data.

Content filter updates are available from Microsoft Update. The content filter update data is updated and available for download every two weeks.

For more information about how to configure content filtering, see [Manage content filtering](manage-content-filtering-exchange-2013-help.md).
