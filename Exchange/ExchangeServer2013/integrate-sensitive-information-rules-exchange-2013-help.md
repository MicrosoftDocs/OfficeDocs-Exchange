---
title: 'Integrating sensitive information rules with transport rules: Exchange 2013 Help'
TOCTitle: Integrating sensitive information rules with transport rules
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: feb014a7-89dd-4f2d-a06d-52806ce435d4
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Integrating sensitive information rules with transport rules in Exchange 2013

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange, you can create DLP policies that contain rules for not only traditional message classifications and existing transport rules but also combine these with rules for sensitive information found within messages. The existing transport rules framework offers rich capabilities to define messaging policies, covering the entire spectrum of soft to hard controls. Examples include:

- Limiting the interaction between recipients and senders, including interactions between departmental groups inside an organization.

- Applying separate policies for communications within and outside of an organization.

- Preventing inappropriate content from entering or leaving an organization.

- Filtering confidential information.

- Tracking or archiving messages that are sent to or received from specific individuals.

- Redirecting inbound and outbound messages for inspection before delivery.

- Applying disclaimers to messages as they pass through the organization.

Transport rules allow you to apply messaging policies to email messages that flow through the transport pipeline in the Transport service on Mailbox servers and on Edge Transport servers. These rules allow system administrators to enforce messaging policies, help keep messages more secure, help to protect messaging systems, and help prevent accidental information loss. For more information about transport rules, see [Transport protection rules](transport-protection-rules-exchange-2013-help.md).

## Sensitive information rules within the transport rule framework

Sensitive information rules are integrated with the transport rules framework by introduction of a condition that you can customize: **If the message contains...Sensitive Information**. This condition can be configured with one or more sensitive information types that are contained within the messages. When multiple DLP policies or rules within a policy are configured with this condition, the policy or rule is satisfied when any of the conditions match. Exchange policy rules examine the subject, body and any attachments of a message. If the rule matches any of these message components, the rule actions will be applied.

The sensitive information condition may be combined with any of the already existing transport rules to define messaging policies. If combined, the condition works in conjunction with other rules and provides the AND semantics. For example, two different conditions are added together with an AND statement such that both need to match for the action to be applied. Any of the transport rule actions can be configured as result of rules containing the sensitive information type matching. Many different file types can be scanned by the transport rules agent, which scans messages to enforce transport rules. To learn more about the supported file types, see [Use transport rules to inspect message attachments](use-transport-rules-to-inspect-message-attachments-exchange-2013-help.md).

The rules can also be used in the exception part of a rule definition. Their use in the exception definition is independent of their use as a condition within the rule. This provides the flexibility to define rules that have the condition specifying multiple information types to be applied as part of the condition and also differing information types in the condition. This would allow policies such as matching specific traditional message-classification rules, but not matching other sensitive information types before performing actions that you define within a policy.

## For more information

[Data loss prevention](data-loss-prevention-exchange-2013-help.md)

[Sensitive information types in Exchange Server](https://docs.microsoft.com/Exchange/policy-and-compliance/data-loss-prevention/sensitive-information-types)

[Transport protection rules](transport-protection-rules-exchange-2013-help.md)

[Create a custom DLP policy](create-custom-dlp-policy-exchange-2013-help.md)
