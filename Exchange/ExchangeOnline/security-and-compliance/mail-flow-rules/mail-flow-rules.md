---
localization_priority: Normal
description: Learn about mail flow rules in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 743bd525-0ca2-426d-b76c-b4a052bc8886
ms.reviewer: 
f1.keywords:
- NOCSH
title: Mail flow rules (transport rules) in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Mail flow rules (transport rules) in Exchange Online

You can use mail flow rules (also known as transport rules) to identify and take action on messages that flow through your Exchange Online organization. Mail flow rules are similar to the Inbox rules that are available in Outlook and Outlook on the web. The main difference is mail flow rules take action on messages while they're in transit, and not after the message is delivered to the mailbox. Mail flow rules contain a richer set of conditions, exceptions, and actions, which provides you with the flexibility to implement many types of messaging policies.

This article explains the [components](#mail-flow-rule-components) of mail flow rules, and [how they work](#how-mail-flow-rules-are-applied-to-messages).

For steps to create, copy, and manage mail flow rules, see [Manage mail flow rules](manage-mail-flow-rules.md). For each rule, you have the option of enforcing it, testing it, or testing it and notifying the sender. To learn more about the testing options, see [Test a mail flow rule](test-mail-flow-rules.md) and [Policy Tips](../../security-and-compliance/data-loss-prevention/policy-tips.md).

For summary and detail reports about messages that matched mail flow rules, see [Use mail protection reports in Office 365 to view data about malware, spam, and rule detections](../../monitoring/use-mail-protection-reports.md).

To implement specific messaging policies by using mail flow rules, see these topics:

- [Use mail flow rules to inspect message attachments in Office 365](inspect-message-attachments.md)

- [Enable message encryption and decryption in Office 365](enable-encryption-and-decryption.md)

- [Common attachment blocking scenarios for mail flow rules](common-attachment-blocking-scenarios.md)

- [Organization-wide message disclaimers, signatures, footers, or headers in Office 365](disclaimers-signatures-footers-or-headers.md)

- [Use mail flow rules so messages can bypass Clutter](use-rules-to-bypass-clutter.md)

- [Use mail flow rules to route email based on a list of words, phrases, or patterns](use-rules-to-route-email.md)

- [Use mail flow rules to set the spam confidence level (SCL) in messages](https://docs.microsoft.com/office365/SecurityCompliance/use-mail-flow-rules-to-set-the-spam-confidence-level-scl-in-messages)

- [Common message approval scenarios](common-message-approval-scenarios.md)

- [Define rules to encrypt or decrypt email messages](https://go.microsoft.com/fwlink/p/?Linkid=402846)

## Mail flow rule components

A mail flow rule is made of conditions, exceptions, actions, and properties:

- **Conditions**: Identify the messages that you want to apply the actions to. Some conditions examine message header fields (for example, the To, From, or Cc fields). Other conditions examine message properties (for example, the message subject, body, attachments, message size, or message classification). Most conditions require you to specify a comparison operator (for example, equals, doesn't equal, or contains) and a value to match. If there are no conditions or exceptions, the rule is applied to all messages.

 For more information about mail flow rule conditions in Exchange Online, see [Mail flow rule conditions and exceptions (predicates) in Exchange Online](conditions-and-exceptions.md).

- **Exceptions**: Optionally identify the messages that the actions shouldn't apply to. The same message identifiers that are available in conditions are also available in exceptions. Exceptions override conditions and prevent the rule actions from being applied to a message, even if the message matches all of the configured conditions.

- **Actions**: Specify what to do to messages that match the conditions in the rule, and don't match any of the exceptions. There are many actions available, such as rejecting, deleting, or redirecting messages, adding additional recipients, adding prefixes in the message subject, or inserting disclaimers in the message body.

   For more information about mail flow rule actions that are available in Exchange Online, see [Mail flow rule actions in Exchange Online](mail-flow-rule-actions.md).

- **Properties**: Specify other rules settings that aren't conditions, exceptions or actions. For example, when the rule should be applied, whether to enforce or test the rule, and the time period when the rule is active.

   For more information, see the [Mail flow rule properties](#mail-flow-rule-properties) section in this topic.

### Multiple conditions, exceptions, and actions

The following table shows how multiple conditions, condition values, exceptions, and actions are handled in a rule.

|**Component**|**Logic**|**Comments**|
|:-----|:-----|:-----|
|Multiple conditions|AND|A message must match all the conditions in the rule. If you need to match one condition or another, use separate rules for each condition. For example, if you want to add the same disclaimer to messages with attachments and messages that contain specific text, create one rule for each condition. In the EAC, you can easily copy a rule.|
|One condition with multiple values|OR|Some conditions allow you to specify more than one value. The message must match any one (not all) of the specified values. For example, if an email message has the subject Stock price information, and the **The subject includes any of these words** condition is configured to match the words Contoso or stock, the condition is satisfied because the subject contains at least one of the specified values.|
|Multiple exceptions|OR|If a message matches any one of the exceptions, the actions are not applied to the message. The message doesn't have to match all the exceptions.|
|Multiple actions|AND|Messages that match a rule's conditions get all the actions that are specified in the rule. For example, if the actions **Prepend the subject of the message with** and **Add recipients to the Bcc box** are selected, both actions are applied to the message. <br/><br/> Keep in mind that some actions (for example, the **Delete the message without notifying anyone** action) prevent subsequent rules from being applied to a message. Other actions (for example, the **Forward the message**) don't allow additional actions. <br/><br/> You can also set an action on a rule so that when that rule is applied, subsequent rules are not applied to the message.|

### Mail flow rule properties

The following table describes the rule properties that are available in mail flow rules.

|**Property name in the EAC**|**Parameter name in PowerShell**|**Description**|
|:-----|:-----|:-----|
|**Priority**|_Priority_|Indicates the order that the rules are applied to messages. The default priority is based on when the rule is created (older rules have a higher priority than newer rules, and higher priority rules are processed before lower priority rules). <br/><br/> You change the rule priority in the EAC by moving the rule up or down in the list of rules. In the PowerShell, you set the priority number (0 is the highest priority). <br/><br/> For example, if you have one rule to reject messages that include a credit card number, and another one requiring approval, you'll want the reject rule to happen first, and stop applying other rules. <br/><br/> For more information, see [Set the priority of a mail flow rule](manage-mail-flow-rules.md#set-the-priority-of-a-mail-flow-rule).|
|**Mode**|_Mode_|You can specify whether you want the rule to start processing messages immediately, or whether you want to test rules without affecting the delivery of the message (with or without Data Loss Prevention or DLP Policy Tips). <br/><br/> Policy Tips present a brief note in Outlook or Outlook on the web that provides information about possible policy violations to the person that's creating the message. For more information, see [Policy Tips](../../security-and-compliance/data-loss-prevention/policy-tips.md). <br/><br/>  For more information about the modes, see [Test a mail flow rule](test-mail-flow-rules.md).|
|**Activate this rule on the following date** <br/><br/> **Deactivate this rule on the following date**|_ActivationDate_ <br/> _ExpiryDate_|Specifies the date range when the rule is active.|
|**On** check box selected or not selected|New rules:_Enabled_ parameter on the **New-TransportRule** cmdlet. <br/> Existing rules: Use the **Enable-TransportRule** or **Disable-TransportRule** cmdlets. <br/><br/> The value is displayed in the **State** property of the rule.|You can create a disabled rule, and enable it when you're ready to test it. Or, you can disable a rule without deleting it to preserve the settings.|
|**Defer the message if rule processing doesn't complete**|_RuleErrorAction_|You can specify how the message should be handled if the rule processing can't be completed. By default, the rule will be ignored, but you can choose to resubmit the message for processing.|
|**Match sender address in message**|_SenderAddressLocation_|If the rule uses conditions or exceptions that examine the sender's email address, you can look for the value in the message header, the message envelope, or both.|
|**Stop processing more rules**|_StopRuleProcessing_|This is an action for the rule, but it looks like a property in the EAC. You can choose to stop applying additional rules to a message after a rule processes a message.|
|**Comments**|_Comments_|You can enter descriptive comments about the rule.|

## How mail flow rules are applied to messages

All messages that flow through your organization are evaluated against the enabled mail flow rules in your organization. Rules are processed in the order listed on the **Mail flow** \> **Rules** page in EAC, or based on the corresponding _Priority_ parameter value in the PowerShell.

Each rule also offers the option of stopping processing more rules when the rule is matched. This setting is important for messages that match the conditions in multiple mail flow rules (which rule do you want applied to the message? All? Just one?).

### Differences in processing based on message type

There are several types of messages that pass through an organization. The following table shows which messages types can be processed by mail flow rules.

|**Type of message**|**Can a rule be applied?**|
|:-----|:-----|
|**Regular messages**: Messages that contain a single rich text format (RTF), HTML, or plain text message body or a multipart or alternative set of message bodies.|Yes|
|**Office 365 Message Encryption**: Messages encrypted by Office 365 Message Encryption in Office 365. For more information, see [Office 365 Message Encryption](https://go.microsoft.com/fwlink/p/?LinkId=392525).|Rules can always access envelope headers and process messages based on conditions that inspect those headers. <br/><br/> For a rule to inspect or modify the contents of an encrypted message, you need to verify that transport decryption is enabled (Mandatory or Optional; the default is Optional). For more information, see [Enable or disable transport decryption](https://go.microsoft.com/fwlink/p/?linkid=848060). <br/><br/> You can also create a rule that automatically decrypts encrypted messages. For more information, see [Define rules to encrypt or decrypt email messages](https://go.microsoft.com/fwlink/p/?Linkid=402846).|
|**S/MIME encrypted messages**|Rules can only access envelope headers and process messages based on conditions that inspect those headers. <br/><br/> Rules with conditions that require inspection of the message's content, or actions that modify the message's content can't be processed.|
|**RMS protected messages**: Messages that had an Active Directory Rights Management Services (AD RMS) or Azure Rights Management (RMS) policy applied.|Rules can always access envelope headers and process messages based on conditions that inspect those headers. <br/><br/> For a rule to inspect or modify the contents of an RMS protected message, you need to verify that transport decryption is enabled (Mandatory or Optional; the default is Optional). For more information, see [Enable or disable transport decryption](https://go.microsoft.com/fwlink/p/?linkid=848060).|
|**Clear-signed messages**: Messages that have been signed but not encrypted.|Yes|
|**Anonymous messages**: Messages sent by anonymous senders.|Yes|
|**Read reports**: Reports that are generated in response to read receipt requests by senders. Read reports have a message class of `IPM.Note*.MdnRead` or `IPM.Note*.MdnNotRead`.|Yes|

## What else should I know?

- The **Version** or **RuleVersion** property value for a rule isn't important in Exchange Online.

- After you create or modify a mail flow rule, it can take up to 30 minutes for the new or updated rule to be applied to messages.

- You can create a transport rule To Bypass ATP and allow mail to flow without delay from internal senders such as scanners, faxes,
that send attachments that are known to be safe and from a trusted source. It is not generally recommend to skip filtering for all internal messages as a compromised account would be able to potentially send malicious content.

- 

## For more information

[Manage mail flow rules](manage-mail-flow-rules.md)

[Use mail flow rules to inspect message attachments in Office 365](inspect-message-attachments.md)

[Organization-wide Disclaimers, Signatures, Footers, or Headers](https://technet.microsoft.com/library/e45e33c9-e53b-427c-ada5-70901bc399b8.aspx)

[Manage message approval](manage-message-approval.md)

[Mail flow rule procedures in Exchange Online](mail-flow-rule-procedures.md)

[Transport and Inbox rule limits](https://go.microsoft.com/fwlink/p/?LinkId=324584)
