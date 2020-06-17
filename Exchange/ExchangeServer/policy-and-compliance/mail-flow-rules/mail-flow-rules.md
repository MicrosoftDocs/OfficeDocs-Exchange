---
localization_priority: Normal
description: Learn about mail flow rules (transport rules) and their components in Exchange 2016 and Exchange 2019.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c3d2031c-fb7b-4866-8ae1-32928d0138ef
ms.reviewer:
title: Mail flow rules in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Mail flow rules in Exchange Server

You can use mail flow rules (also known as transport rules) to identify and take action on messages that flow through the transport pipeline in your Exchange 2016 and Exchange 2019 organization. Mail flow rules are similar to the Inbox rules that are available in Outlook and Outlook on the web (formerly known as Outlook Web App). The main difference is mail flow rules take action on messages while they're in transit, and not after the message is delivered to the mailbox. Mail flow rules contain a richer set of conditions, exceptions, and actions, which provides you with the flexibility to implement many types of messaging policies.

This article explains the [components](#mail-flow-rule-components) of mail flow rules, and [how they work](#how-mail-flow-rules-are-applied).

You can use the Exchange admin center (EAC) or the Exchange Management Shell to manage mail flow rules. For instructions on how to manage mail flow rules, see [Procedures for mail flow rules in Exchange Server](mail-flow-rule-procedures.md).

 For each rule, you have the option of enforcing it, testing it, or testing it and notifying the sender. To learn more about the testing options, see [Test a mail flow rule](https://docs.microsoft.com/Exchange/test-transport-rules-exchange-2013-help) and [Policy Tips](https://docs.microsoft.com/Exchange/policy-tips-exchange-2013-help).

For steps to implement specific messaging policies, see the following topics:

- [Organization-wide disclaimers, signatures, footers, or headers in Exchange Server](signatures.md)

- [Common message approval scenarios](https://docs.microsoft.com/Exchange/common-message-approval-scenarios-exchange-2013-help)

- [Using mail flow rules to inspect message attachments](https://docs.microsoft.com/exchange/use-transport-rules-to-inspect-message-attachments-exchange-2013-help)

## Mail flow rule components

A rule is made of conditions, exceptions, actions, and properties:

- **Conditions**: Identify the messages that you want to apply the actions to. Some conditions examine message header fields (for example, the To, From, or Cc fields). Other conditions examine message properties (for example, the message subject, body, attachments, message size, or message classification). Most conditions require you to specify a comparison operator (for example, equals, doesn't equal, or contains) and a value to match. If there are no conditions or exceptions, the rule is applied to all messages.

    For a complete list of mail flow rule conditions, see [Mail flow rule conditions and exceptions (predicates) in Exchange Server](conditions-and-exceptions.md).

- **Exceptions**: Optionally identify the messages that the actions shouldn't apply to. The same message identifiers that are available in conditions are also available in exceptions. Exceptions override conditions and prevent the rule actions from being applied to a message, even if the message matches all of the configured conditions.

- **Actions**: Specify what to do to messages that match the conditions in the rule, and don't match any of the exceptions. There are many actions available, such as rejecting, deleting, or redirecting messages, adding additional recipients, adding prefixes in the message subject, or inserting disclaimers in the message body.

    For a complete list of mail flow rule actions available, see [Mail flow rule actions in Exchange Server](actions.md).

- **Properties**: Specify other rules settings that aren't conditions, exceptions or actions. For example, when the rule should be applied, whether to enforce or test the rule, and the time period when the rule is active. For more information, see the [Mail flow rule properties](#mail-flow-rule-properties) section in this topic.

### Multiple conditions, exceptions, and actions

The following table shows how multiple conditions, condition values, exceptions, and actions are handled in a rule.

|**Component**|**Logic**|**Comments**|
|:-----|:-----|:-----|
|Multiple conditions|AND|A message must match all the conditions in the rule. If you need to match one condition or another, use separate rules for each condition. For example, if you want to add the same disclaimer to messages with attachments and messages that contain specific text, create one rule for each condition. In the EAC, you can easily copy a rule.|
|One condition with multiple values|OR|Some conditions allow you to specify more than one value. The message must match any one (not all) of the specified values. For example, if an email message has the subject Stock price information, and the **The subject includes any of these words** condition is configured to match the words Contoso or stock, the condition is satisfied because the subject contains at least one of the specified values.|
|Multiple exceptions|OR|If a message matches any one of the exceptions, the actions are not applied to the message. The message doesn't have to match all the exceptions.|
|Multiple actions|AND|Messages that match a rule's conditions get all the actions that are specified in the rule. For example, if the actions **Prepend the subject of the message with** and **Add recipients to the Bcc box** are selected, both actions are applied to the message. <br/> Keep in mind that some actions, such as the **Delete the message without notifying anyone** action, prevent subsequent rules from being applied to a message. Other actions such as **Forward the message** do not allow additional actions. <br/> You can also set an action on a rule so that when that rule is applied, subsequent rules are not applied to the message.|

### Mail flow rule properties

The following table describes the rule properties that are available in mail flow rules.

|**Property name in the EAC**|**Parameter name in the Exchange Management Shell**|**Description**|
|:-----|:-----|:-----|
|**Priority**|_Priority_|Indicates the order that the rules are applied to messages. The default priority is based on when the rule is created (older rules have a higher priority than newer rules), and higher priority rules are processed before lower priority rules. <br/> You change the rule priority in the EAC by moving the rule up or down in the list of rules. In the Exchange Management Shell, you set the priority number (0 is the highest priority). <br/> For example, if you have one rule to reject messages that include a credit card number, and another one requiring approval, you'll want the reject rule to happen first, and stop applying other rules. <br/> For more information, see [Set the priority of mail flow rules](mail-flow-rule-procedures.md#set-the-priority-of-mail-flow-rules).|
|**Mode**| _Mode_|You can specify whether you want the rule to start processing messages immediately, or whether you want to test rules without affecting the delivery of the message (with or without Data Loss Prevention or DLP Policy Tips). <br/> Policy Tips are similar to MailTips, and can be configured to present a brief note in Outlook or Outlook on the web that provides information about possible policy violations to the person that's creating the message. For more information, see [Policy Tips](https://docs.microsoft.com/Exchange/policy-tips-exchange-2013-help).For more information about the modes, see [Test a mail flow rule](https://docs.microsoft.com/Exchange/test-transport-rules-exchange-2013-help).|
|**Activate this rule on the following date** <br/> **Deactivate this rule on the following date**| _ActivationDate_ <br/> _ExpiryDate_|Specifies the date range when the rule is active.|
|**On** check box selected or not selected|New rules: _Enabled_ parameter on the **New-TransportRule** cmdlet. <br/> Existing rules: Use the **Enable-TransportRule** or **Disable-TransportRule** cmdlets. <br/> The value is displayed in the **State** property of the rule.|You can create a disabled rule, and enable it when you're ready to test it. Or, you can disable a rule without deleting it to preserve the settings. For instructions, see [Enable or disable mail flow rules](mail-flow-rule-procedures.md#enable-or-disable-mail-flow-rules).|
|**Defer the message if rule processing doesn't complete**| _RuleErrorAction_|You can specify how the message should be handled if the rule processing can't be completed. By default, the rule will be ignored, but you can choose to resubmit the message for processing.|
|**Match sender address in message**| _SenderAddressLocation_|If the rule uses conditions or exceptions that examine the sender's email address, you can look for the value in the message header, the message envelope, or both. For more information, see [Senders](conditions-and-exceptions.md#senders).|
|**Stop processing more rules**| _SenderAddressLocation_|This is an action for the rule, but it looks like a property in the EAC. You can choose to stop applying additional rules to a message after a rule processes a message.|
|**Comments**| _Comments_|**Comments** You can enter descriptive comments about the rule.|

## How mail flow rules are applied

Mail flow rules are applied by a transport agent on Mailbox servers and Edge Transport servers. On Mailbox servers, rules are applied by the Transport Rule agent. On Edge Transport servers, rules are applied by Edge Rule agent. Although similar in functionality, the agents have some differences. The important differences are summarized in the following table:

|**Transport agent**|**SMTP or categorizer event where rules are invoked**|**Where rules are stored**|
|:-----|:-----|:-----|
|**Transport Rule agent on Mailbox servers**|The **OnResolvedMessage** categorizer event. <br/> In Exchange 2010, the Transport Rule agent was invoked on the **OnRoutedMessage** categorizer event. The change to **OnResolvedMessage** allowed new rule actions that can change how a message is routed (for example, require TLS).|In Active Directory. Rules are available to all Mailbox servers in the Active Directory forest.|
|**Edge Rule agent on Edge Transport servers**|The **OnEndOfData** SMTP event|In the local instance of Active Directory Lightweight Directory Services (AD LDS) on the server. Rules are only applied to messages that flow through the local server.|

For more information about transport agents, see [Transport Agents](https://docs.microsoft.com/exchange/transport-agents-exchange-2013-help).

### Differences in processing based on message type

There are several types of messages that flow through an organization. The following table shows which messages types can be processed by mail flow rules.

****

|**Type of message**|**Can a rule be applied?**|
|:-----|:-----|
|**Regular messages** Messages that contain a single rich text format (RTF), HTML, or plain text message body or a multipart or alternative set of message bodies. |Yes |
|**S/MIME encrypted messages**|Rules can only access envelope headers and process messages based on conditions that inspect those headers. <br/> Rules with conditions that require inspection of the message's content, or actions that modify the message's content can't be processed. |
|**RMS Protected messages**: Messages that are protected by applying an Active Directory Rights Management Services (AD RMS) rights policy template.|Rules can always access envelope headers and process messages based on conditions that inspect those headers.For a rule to inspect or modify a protected message's content, your need to: <br/>• Have transport decryption set to **Mandatory** or **Optional**. By default, Transport decryption is set to **Optional**. <br/>• Have the encryption key.|
|**Clear-signed messages**: Messages that have been signed but not encrypted.|Yes|
|**UM messages**: Messages that are created or processed by the Unified Messaging service in Exchange 2016, such as voice mail, fax, missed call notifications, and messages created or forwarded by using Microsoft Outlook Voice Access. (**Note**: Unified Messaging is not available in Exchange 2019.)|Yes|
|**Anonymous messages**: Messages that were sent by anonymous senders.|Yes|
|**Read reports**: Reports that are generated in response to read receipt requests by senders. Read reports have a message class of `IPM.Note*.MdnRead` or `IPM.Note*.MdnNotRead`.|Yes|

### Rule storage and replication

Mail flow rules that you create and configure on Mailbox servers are stored in Active Directory, and they're read and applied by the Transport service on all Mailbox servers in the organization. When you create, modify, or remove a mail flow rule, the change is replicated between the domain controllers in your organization. This allows Exchange to provide a consistent set of mail flow rules across the organization.

 **Notes**:

- Replication between domain controllers depends on factors that aren't controlled by Exchange (for example, the number of Active Directory sites, and the speed of network links). Therefore, you need to consider replication delays when you implement mail flow rules in your organization. For more information about Active Directory replication, see [Introduction to Active Directory Replication and Topology Management Using Windows PowerShell](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/powershell/introduction-to-active-directory-replication-and-topology-management-using-windows-powershell--level-100-).

- Each Mailbox server caches expanded distribution groups to avoid repeated Active Directory queries to determine a group's membership. By default, entries in the expanded groups cache expire every four hours. Therefore, changes to the group's membership aren't detected by mail flow rules until the expanded groups cache is updated. To force an immediate update of the cache on a Mailbox server, restart the Microsoft Exchange Transport service. You need to restart the service on each Mailbox server where you want to forcibly update the cache.

Mail flow rules that you create and configure on Edge Transport servers are stored in the local instance of AD LDS on the server. No automated replication of mail flow rules occurs on Edge Transport servers. Rules on the Edge Transport server apply only to messages that flow through the local server. If you need to apply the same set of mail flow rules on multiple Edge Transport servers, you can clone the Edge Transport server configuration, or export and import the mail flow rules. For more information, see [Edge Transport Server Cloned Configuration](https://docs.microsoft.com/exchange/edge-transport-server-cloned-configuration-exchange-2013-help) and [Import or export mail flow rule collections](mail-flow-rule-procedures.md#import-or-export-mail-flow-rule-collections).

Whenever the Transport service on a Mailbox server or Edge Transport server detects a modified mail flow rule, an event is logged in the Application log in the Event Viewer (Event ID 4002 on Mailbox servers, and Event ID 16028 on Edge Transport servers).

#### Rule replication and storage in mixed environments

There are two mixed environment scenarios that are common:

- **Hybrid deployments where part of your organization resides in Office 365**

  In a hybrid environment, there's no replication of rules between your on-premises Exchange organization and Office 365. Therefore, when you create a rule in Exchange, you need to create a matching rule in Office 365. Rules you create in Office 365 are stored in the cloud, whereas the rules you create in your on-premises organization are stored locally in Active Directory. When you manage rules in a hybrid environment, you need to keep the two sets of rules synchronized by making the change in both places, or making the change in one environment and then exporting the rules and importing them in the other environment.

  **Important**: Even though there is a substantial overlap between the conditions and actions that are available in Office 365 and Exchange Server, there are differences. If you plan on creating the same rule in both locations, make sure that all conditions and actions you plan to use are available. To see the list of available conditions and actions that are available in Office 365, see the following topics:

  [Mail flow rule conditions and exceptions (predicates) in Exchange Online](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/conditions-and-exceptions)

  [Mail flow rule actions in Exchange Online](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/mail-flow-rule-actions)

- **Coexistence with Exchange 2010**

  > [!NOTE]
  > This section applies to Exchange 2016 only.

  When you coexist with Exchange 2010, all mail flow rules are stored in Active Directory and replicated across your organization regardless of the Exchange Server version you used to create the rules. However, all mail flow rules are associated with the Exchange server version that was used to create them and are stored in a version-specific container in Active Directory. When you first deploy Exchange 2016 in your organization, any existing rules are imported to Exchange 2016 as part of the setup process. However, any changes afterwards would need to be made with both versions. For example, if you change an existing rule in Exchange 2016 (Exchange Management Shell or the EAC), you need to make the same change in Exchange 2010 (Exchange Management Shell or the Exchange Management Console).

  Exchange 2010 can't process rules that have the **Version** or **RuleVersion** value 15._n_._n_._n_. To be sure all your rules can be processed, only use rules that have the value 14._n_._n_._n_.
