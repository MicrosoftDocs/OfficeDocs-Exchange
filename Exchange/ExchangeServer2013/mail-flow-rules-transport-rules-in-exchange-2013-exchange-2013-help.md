---
title: 'Transport rules in Exchange 2013: Exchange 2013 Help'
TOCTitle: Transport rules
ms:assetid: c3d2031c-fb7b-4866-8ae1-32928d0138ef
ms:mtpsurl: https://technet.microsoft.com/library/Dd351127(v=EXCHG.150)
ms:contentKeyID: 49289403
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Transport rules in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use transport rules to identify and take action on messages that flow through your Exchange 2013 organization. Transport rules are similar to the Inbox rules that are available in Outlook and Outlook Web App. The main difference is transport rules take action on messages while they're in transit, and not after the message is delivered to the mailbox. Transport rules contain a richer set of conditions, exceptions, and actions, which provides you with the flexibility to implement many types of messaging policies.

This article explains the components of transport rules, and how they work.

You can use the Exchange admin center (EAC) or the Exchange Management Shell to manage transport rules. For instructions on how to manage transport rules, see [Manage transport rules in Exchange 2013](manage-transport-rules-exchange-2013-help.md).

For each rule, you have the option of enforcing it, testing it, or testing it and notifying the sender. To learn more about the testing options, see [Test a transport rule](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/test-mail-flow-rules) and [Policy Tips](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/policy-tips).

To implement specific messaging policies by using transport rules, see these topics:

- [Use transport rules to inspect message attachments in Exchange 2013](use-transport-rules-to-inspect-message-attachments-exchange-2013-help.md)

- [Common attachment blocking scenarios for transport rules](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/common-attachment-blocking-scenarios)

- [Organization-wide disclaimers, signatures, footers, or headers in Exchange 2013](organization-wide-disclaimers-signatures-footers-or-headers-exchange-2013-help.md)

- [Use transport rules so messages can bypass Clutter](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/use-rules-to-bypass-clutter)

- [Use transport rules to route email based on a list of words, phrases, or patterns](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/use-rules-to-route-email)

- [Common message approval scenarios](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/common-message-approval-scenarios)

## Transport rule components

A transport rule is made of conditions, exceptions, actions, and properties:

- **Conditions**: Identify the messages that you want to apply the actions to. Some conditions examine message header fields (for example, the To, From, or Cc fields). Other conditions examine message properties (for example, the message subject, body, attachments, message size, or message classification). Most conditions require you to specify a comparison operator (for example, equals, doesn't equal, or contains) and a value to match. If there are no conditions or exceptions, the rule is applied to all messages.

    For more information about transport rule conditions in Exchange 2013, see [Transport rule conditions and exceptions (predicates) in Exchange 2013](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md).

- **Exceptions**: Optionally identify the messages that the actions shouldn't apply to. The same message identifiers that are available in conditions are also available in exceptions. Exceptions override conditions and prevent the rule actions from being applied to a message, even if the message matches all of the configured conditions.

- **Actions**: Specify what to do to messages that match the conditions in the rule, and don't match any of the exceptions. There are many actions available, such as rejecting, deleting, or redirecting messages, adding additional recipients, adding prefixes in the message subject, or inserting disclaimers in the message body.

    For more information about transport rule actions in Exchange 2013, see [Transport rule actions in Exchange 2013](mail-flow-rule-actions-in-exchange-2013-exchange-2013-help.md).

- **Properties**: Specify other rules settings that aren't conditions, exceptions or actions. For example, when the rule should be applied, whether to enforce or test the rule, and the time period when the rule is active.

    For more information, see the Transport rule properties section in this topic.

## Multiple conditions, exceptions, and actions

The following table shows how multiple conditions, condition values, exceptions, and actions are handled in a rule.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Component</th>
<th>Logic</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Multiple conditions</p></td>
<td><p>AND</p></td>
<td><p>A message must match all the conditions in the rule. If you need to match one condition or another, use separate rules for each condition. For example, if you want to add the same disclaimer to messages with attachments and messages that contain specific text, create one rule for each condition. In the EAC, you can easily copy a rule.</p></td>
</tr>
<tr class="even">
<td><p>One condition with multiple values</p></td>
<td><p>OR</p></td>
<td><p>Some conditions allow you to specify more than one value. The message must match any one (not all) of the specified values. For example, if an email message has the subject <strong>Stock price information</strong>, and the <strong>The subject includes any of these words</strong> condition is configured to match the words <strong>Contoso</strong> or <strong>stock</strong>, the condition is satisfied because the subject contains at least one of the specified values.</p></td>
</tr>
<tr class="odd">
<td><p>Multiple exceptions</p></td>
<td><p>OR</p></td>
<td><p>If a message matches any one of the exceptions, the actions are not applied to the message. The message doesn't have to match all the exceptions.</p></td>
</tr>
<tr class="even">
<td><p>Multiple actions</p></td>
<td><p>AND</p></td>
<td><p>Messages that match a rule's conditions get all the actions that are specified in the rule. For example, if the actions <strong>Prepend the subject of the message with</strong> and <strong>Add recipients to the Bcc box</strong> are selected, both actions are applied to the message.</p>
<p>Keep in mind that some actions, such as the <strong>Delete the message without notifying anyone</strong> action, prevent subsequent rules from being applied to a message. Other actions such as <strong>Forward the message</strong> do not allow additional actions.</p>
<p>You can also set an action on a rule so that when that rule is applied, subsequent rules are not applied to the message.</p></td>
</tr>
</tbody>
</table>

## Transport rule properties

The following table describes the rule properties that are available in transport rules.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Property name in the EAC</th>
<th>Parameter name in PowerShell</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Priority</strong></p></td>
<td><p><em>Priority</em></p></td>
<td><p>Indicates the order that the rules are applied to messages. The default priority is based on when the rule is created (older rules have a higher priority than newer rules, and higher priority rules are processed before lower priority rules).</p>
<p>You change the rule priority in the EAC by moving the rule up or down in the list of rules. In the PowerShell, you set the priority number (0 is the highest priority).</p>
<p>For example, if you have one rule to reject messages that include a credit card number, and another one requiring approval, you'll want the reject rule to happen first, and stop applying other rules.</p>
<p>For more information, see [Set the priority of a transport rule](manage-transport-rules-exchange-2013-help.md#set-the-priority-of-a-transport-rule).</p></td>
</tr>
<tr class="even">
<td><p><strong>Mode</strong></p></td>
<td><p><em>Mode</em></p></td>
<td><p>You can specify whether you want the rule to start processing messages immediately, or whether you want to test rules without affecting the delivery of the message (with or without Data Loss Prevention or DLP Policy Tips).</p>
<p>Policy Tips present a brief note in Outlook or Outlook on the web that provides information about possible policy violations to the person that's creating the message. For more information, see <a href="https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/policy-tips">Policy Tips</a>.</p>
<p>For more information about the modes, see <a href="https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/test-mail-flow-rules">Test a transport rule</a>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Activate this rule on the following date</strong></p>
<p><strong>Deactivate this rule on the following date</strong></p></td>
<td><p><em>ActivationDate</em></p>
<p><em>ExpiryDate</em></p></td>
<td><p>Specifies the date range when the rule is active.</p></td>
</tr>
<tr class="even">
<td><p><strong>On</strong> check box selected or not selected</p></td>
<td><p>New rules: <em>Enabled</em> parameter on the <strong>New-TransportRule</strong> cmdlet.</p>
<p>Existing rules: Use the <strong>Enable-TransportRule</strong> or <strong>Disable-TransportRule</strong> cmdlets.</p>
<p>The value is displayed in the <strong>State</strong> property of the rule.</p></td>
<td><p>You can create a disabled rule, and enable it when you're ready to test it. Or, you can disable a rule without deleting it to preserve the settings.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Defer the message if rule processing doesn't complete</strong></p></td>
<td><p><em>RuleErrorAction</em></p></td>
<td><p>You can specify how the message should be handled if the rule processing can't be completed. By default, the rule will be ignored, but you can choose to resubmit the message for processing.</p></td>
</tr>
<tr class="even">
<td><p><strong>Match sender address in message</strong></p></td>
<td><p><em>SenderAddressLocation</em></p></td>
<td><p>If the rule uses conditions or exceptions that examine the sender's email address, you can look for the value in the message header, the message envelope, or both.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Stop processing more rules</strong></p></td>
<td><p><em>SenderAddressLocation</em></p></td>
<td><p>This is an action for the rule, but it looks like a property in the EAC. You can choose to stop applying additional rules to a message after a rule processes a message.</p></td>
</tr>
<tr class="even">
<td><p><strong>Comments</strong></p></td>
<td><p><em>Comments</em></p></td>
<td><p>You can enter descriptive comments about the rule.</p></td>
</tr>
</tbody>
</table>

## How transport rules are applied to messages

UNRESOLVED\_TOKENBLOCK\_VAL(GENL\_TransportRules\_HowApplied)

## Differences in processing based on message type

There are several types of messages that pass through an organization. The following table shows which messages types can be processed by transport rules.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Type of message</th>
<th>Can a rule be applied?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Regular messages</strong>   Messages that contain a single rich text format (RTF), HTML, or plain text message body or a multipart or alternative set of message bodies.</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p><strong>Office 365 Message Encryption</strong>    Messages encrypted by Office 365 Message Encryption in Office 365. For more information, see <a href="https://docs.microsoft.com/microsoft-365/compliance/encryption">Office 365 Message Encryption</a>.</p></td>
<td><p>Rules can always access envelope headers and process messages based on conditions that inspect those headers.</p>
<p>For a rule to inspect or modify the contents of an encrypted message, you need to verify that transport decryption is enabled (Mandatory or Optional; the default is Optional). For more information, see <a href="https://docs.microsoft.com/exchange/enable-or-disable-transport-decryption-exchange-2013-help">Enable or disable transport decryption</a>.</p>
<p>You can also create a rule that automatically decrypts encrypted messages. For more information, see <a href="https://docs.microsoft.com/microsoft-365/compliance/define-mail-flow-rules-to-encrypt-email">Define mail flow rules to encrypt email messages</a>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>S/MIME encrypted messages</strong></p></td>
<td><p>Rules can only access envelope headers and process messages based on conditions that inspect those headers.</p>
<p>Rules with conditions that require inspection of the message's content, or actions that modify the message's content can't be processed.</p></td>
</tr>
<tr class="even">
<td><p><strong>RMS protected messages</strong>   Messages that had an Active Directory Rights Management Services (AD RMS) or Azure Rights Management (RMS) policy applied.</p></td>
<td><p>Rules can always access envelope headers and process messages based on conditions that inspect those headers.</p>
<p>For a rule to inspect or modify the contents of an RMS protected message, you need to verify that transport decryption is enabled (Mandatory or Optional; the default is Optional). For more information, see <a href="https://docs.microsoft.com/exchange/enable-or-disable-transport-decryption-exchange-2013-help">Enable or disable transport decryption</a>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Clear-signed messages</strong>   Messages that have been signed but not encrypted.</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p><strong>UM messages</strong>   Messages that are created or processed by the Unified Messaging service, such as voice mail, fax, missed call notifications, and messages created or forwarded by using Microsoft Outlook Voice Access.</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p><strong>Anonymous messages</strong>   Messages sent by anonymous senders.</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p><strong>Read reports</strong>   Reports that are generated in response to read receipt requests by senders. Read reports have a message class of <code>IPM.Note*.MdnRead</code> or <code>IPM.Note*.MdnNotRead</code>.</p></td>
<td><p>Yes</p></td>
</tr>
</tbody>
</table>

## Transport rules and group membership

When you define a transport rule using a condition that expands membership of a distribution group, the resulting list of recipients is cached by the Transport service on the Mailbox server that applies the rule. This is known as the *Expanded Groups Cache* and is also used by the Journaling agent for evaluating group membership for journal rules. By default, the Expanded Groups Cache stores group membership for four hours. Recipients returned by the recipient filter of a dynamic distribution group are also stored. The Expanded Groups Cache makes repeated round-trips to Active Directory and the resulting network traffic from resolving group memberships unnecessary.

In Exchange 2013, this interval and other parameters related to the Expanded Groups Cache are configurable. You can lower the cache expiration interval, or disable caching altogether, to ensure group memberships are refreshed more frequently. You must plan for the corresponding increase in load on your Active Directory domain controllers for distribution group expansion queries. You can also clear the cache on a Mailbox server by restarting the Microsoft Exchange Transport service on that server. You must do this on each Mailbox server where you want to clear the cache. When creating, testing, and troubleshooting transport rules that use conditions based on distribution group membership, you must also consider the impact of Expanded Groups Cache.

## Rule storage and replication

Transport rules that you create and configure on Mailbox servers are stored in Active Directory, and they're read and applied by the Transport service on all Mailbox servers in the organization. When you create, modify, or remove a transport rule, the change is replicated between the domain controllers in your organization. This allows Exchange to provide a consistent set of transport rules across the organization.

**Notes**:

- Replication between domain controllers depends on factors that aren't controlled by Exchange (for example, the number of Active Directory sites, and the speed of network links). Therefore, you need to consider replication delays when you implement transport rules in your organization. For more information about Active Directory replication, see [Introduction to Active Directory Replication and Topology Management Using Windows PowerShell](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/powershell/introduction-to-active-directory-replication-and-topology-management-using-windows-powershell--level-100-).

- Each Mailbox server caches expanded distribution groups to avoid repeated Active Directory queries to determine a group's membership. By default, entries in the expanded groups cache expire every four hours. Therefore, changes to the group's membership aren't detected by transport rules until the expanded groups cache is updated. To force an immediate update of the cache on a Mailbox server, restart the Microsoft Exchange Transport service. You need to restart the service on each Mailbox server where you want to forcibly update the cache.

Transport rules that you create and configure on Edge Transport servers are stored in the local instance of AD LDS on the server. No automated replication of transport rules occurs on Edge Transport servers. Rules on the Edge Transport server apply only to messages that flow through the local server. If you need to apply the same set of transport rules on multiple Edge Transport servers, you can clone the Edge Transport server configuration, or export and import the transport rules. For more information, see [Edge Transport server cloned configuration](edge-transport-server-cloned-configuration-exchange-2013-help.md) and [Import or export a transport rule collection](manage-transport-rules-exchange-2013-help.md#import-or-export-a-transport-rule-collection).

Whenever the Transport service on a Mailbox server or Edge Transport server detects a modified transport rule, an event is logged in the Application log in the Event Viewer (Event ID 4002 on Mailbox servers, and Event ID 16028 on Edge Transport servers).

## Rule replication and storage in mixed environments

There are two mixed environment scenarios that are common in Exchange 2013:

- **Hybrid deployments where part of your organization resides in Office 365**

  In a hybrid environment, there's no replication of rules between your on-premises Exchange organization and Office 365. Therefore, when you create a rule in Exchange, you need to create a matching rule in Office 365. Rules you create in Office 365 are stored in the cloud, whereas the rules you create in your on-premises organization are stored locally in Active Directory. When you manage rules in a hybrid environment, you need to keep the two sets of rules synchronized by making the change in both places, or making the change in one environment and then exporting the rules and importing them in the other environment.

  > [!IMPORTANT]
  > Even though there is a substantial overlap between the conditions and actions that are available in Office 365 and Exchange Server, there are differences. If you plan on creating the same rule in both locations, make sure that all conditions and actions you plan to use are available. To see the list of available conditions and actions that are available in Office 365, see the following topics:<BR><A href="https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/conditions-and-exceptions">Transport rule conditions and exceptions (predicates) in Exchange Online</A><BR><A href="https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/mail-flow-rule-actions">Mail flow rule actions in Exchange Online</A>

- **Coexistence with Exchange 2010 or Exchange 2007**

  When you coexist with Exchange 2010 or Exchange 2007, all transport rules are stored in Active Directory and replicated across your organization, regardless of the Exchange Server version you used to create the rules. However, all transport rules are associated with the Exchange Server server version that was used to create them, and are stored in a version-specific container in Active Directory. When you first deploy Exchange 2013 in your organization, any existing rules are imported to Exchange 2013 as part of the setup process. However, any changes afterwards would need to be made with both versions. For example, if you change an existing rule in Exchange 2013 (Exchange Management Shell or the EAC), you need to make the same change in Exchange 2010 (Exchange Management Shell or the UNRESOLVED\_TOKEN\_VAL(exEMC)). Or, you can export the rules from Exchange 2013 and import them into Exchange 2010.

  Exchange 2010 can't process rules that have the **Version** or **RuleVersion** value 15.*n*.*n*.*n*. To be sure all your rules can be processed, only use rules that have the value 14.*n*.*n*.*n*.

## For more information

[Manage transport rules in Exchange 2013](manage-transport-rules-exchange-2013-help.md)

[Transport rule conditions and exceptions (predicates) in Exchange 2013](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md)

[Transport rule actions in Exchange 2013](mail-flow-rule-actions-in-exchange-2013-exchange-2013-help.md)

[Transport agents](transport-agents-exchange-2013-help.md)
