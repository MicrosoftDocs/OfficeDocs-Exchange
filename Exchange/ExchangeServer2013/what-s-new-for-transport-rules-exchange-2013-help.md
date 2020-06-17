---
title: "What's new for transport rules: Exchange 2013 Help"
TOCTitle: What's new for transport rules
ms:assetid: 0c2fc0b5-3cd2-4d79-aa2b-0c7622ae15a8
ms:mtpsurl: https://technet.microsoft.com/library/JJ150483(v=EXCHG.150)
ms:contentKeyID: 47559940
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# What's new for transport rules

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, several improvements have been made to transport rules. This topic provides a brief overview of some of the key changes and enhancements. To learn more about transport rules, see [Mail flow or transport rules](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md).

## Support for data loss prevention policies

Data loss prevention (DLP) features in Exchange 2013 can help organizations reduce unintentional disclosure of sensitive data. Transport rules have been updated to support creating rules that accompany and enforce DLP policies. To learn more about DLP support in transport rules, see the following topics:

[Integrating sensitive information rules with transport rules](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/integrate-sensitive-information-rules)

[Data loss prevention](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention)

## New predicates and actions

The functionality of transport rules has been extended via the addition of new predicates and actions. Each predicate listed below can be used as a condition or an exception when you're creating transport rules.

For detailed information about using these new predicates and actions, see [Transport rule conditions (predicates)](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md) and [Transport rule actions](mail-flow-rule-actions-in-exchange-2013-exchange-2013-help.md).

## New predicates

- **AttachmentExtensionMatchesWords**: Used to detect messages that contain attachments with specific extensions.

- **AttachmentHasExecutableContent**: Used to detect messages that contain attachments with executable content.

- **HasSenderOverride**: Used to detect messages where the sender has chosen to override a DLP policy restriction.

- **MessageContainsDataClassifications**: Used to detect sensitive information in the message body and any of the attachments. For a list of data classifications available, see [What the sensitive information types in Exchange 2013 look for](what-the-sensitive-information-types-in-exchange-look-for-exchange-2013-help.md).

- **MessageSizeOver**: Used to detect messages whose overall size is greater than or equal to the specified limit.

- **SenderIPRanges**: Used to detect messages sent from a specific set of IP address ranges.

## New actions

- **GenerateIncidentReport**: Generates an incident report that is sent to a specified SMTP address. The action also has a parameter called *IncidentReportOriginalMail* that accepts one of two values: IncludeOriginalMail or DoNotIncludeOriginalMail.

- **NotifySender**: Controls how the sender of a message that goes against a DLP policy is notified. You can choose to simply inform the sender and route the message normally, or you can choose to reject the message and notify the sender.

- **StopRuleProcessing**: Stops the processing of all subsequent rules on the message.

- **ReportSeverityLevel**: Sets the specified severity level in the incident report. Values for the action are: Informational, Low, Medium, High, and Off.

- **RouteMessageOutboundRequireTLS**: Requires Transport Layer Security (TLS) encryption when routing this message outside your organization. If TLS encryption isn't supported, the message is rejected and not delivered.

## Other changes in Transport rules

  - **Support for extended regular expression syntax**: Transport rules in Exchange 2013 are based on the Microsoft.NET Framework regular expression (regex) functionality and now support extended regular expression syntax.

  - **Transport rules agent invocation**: The key architectural change in Exchange 2013 for Transport rules is the Transport Rules Agent is invoked on onResolvedMessage. In previous versions of Exchange, the Rules Agent was invoked on onRoutedMessage. This change allowed us to add new actions, such as requiring TLS, which can change how a message is routed. To learn more about the transport rules architecture in Exchange 2013, see [Mail flow or transport rules](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md).

  - **Detailed Transport rule information in message tracking logs**: Detailed information about Transport rules is now included in message tracking logs. The information includes which rules were triggered for a specific message and the actions taken as a result of processing those rules.

  - **New rule monitoring functionality**: Exchange 2013 monitors Transport rules that are configured and measures the cost of running these rules both when you're creating the rule and also during regular operation. Exchange can detect and generate alerts for rules that are causing delays in mail delivery.
