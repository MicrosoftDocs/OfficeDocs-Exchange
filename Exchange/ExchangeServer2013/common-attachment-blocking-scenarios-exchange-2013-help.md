---
title: 'Common attachment blocking scenarios for transport rules: Exchange 2013 Help'
TOCTitle: Common attachment blocking scenarios for transport rules
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 5c576439-d55b-4c7f-90ed-a7f72cbb16c2
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Common attachment blocking scenarios for transport rules in Exchange 2013

_**Applies to:** Exchange Server 2013_

Your organization might require that certain types of messages be blocked or rejected in order to meet legal or compliance requirements, or to implement specific business needs. This article discusses examples of common scenarios for blocking all attachments which you can set up using transport rules in Exchange.

For additional examples showing how to block specific attachments, see [Use transport rules to inspect message attachments](use-transport-rules-to-inspect-message-attachments-exchange-2013-help.md).

The malware filter includes a Common Attachment Types Filter. In the Exchange admin center (EAC), go to **Protection**, then click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif) to add filters.

To get started implementing any of these scenarios to block certain message types:

1. Sign in to the Exchange admin center.

2. Go to **Mail flow** \> **Rules**.

3. Click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif) and then select **Create a new rule**.

4. In the **Name** box, specify a name for the rule, and then click **More options**.

5. Select the conditions and actions you want.

   **Note**: In the EAC, the smallest attachment size that you can enter is 1 kilobyte, which should detect most attachments. However, if you want to detect every possible attachment of any size, you need to use PowerShell to adjust the attachment size to 1 byte after you create the rule in the EAC. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

   Replace _\<Rule Name\>_ with the name of the existing rule, and run the following command to set the attachment size to 1 byte:

   ```powershell
   Set-TransportRule -Identity "<Rule Name>" -AttachmentSizeOver 1B
   ```

After you adjust the attachment size to 1 byte, the value that's displayed for the rule in the EAC is **0.00 KB**.

## Example 1: Block messages with attachments, and notify the sender

If you don't want people in your organization to send or receive attachments, you can set up a transport rule to block all messages with attachments.

In this example, all messages sent to or from the organization with attachments are blocked.

![Rule that blocks all attachments](images/38094183-166f-4ba5-a9cf-242e7d0f4e04.png)

If all you want to do is block the message, you might want to stop rule processing once this rule is matched. Scroll down the rule dialog box, and select the **Stop processing more rules** check box.

## Example 2: Notify intended recipients when an inbound message is blocked

If you want to reject a message but let the intended recipient know what happened, you can use the **Notify the recipient with a message** action.

You can include placeholders in the notification message so that it includes information about the original message. The placeholders must be enclosed in two percent signs (%%), and when the notification message is sent, the placeholders are replaced with information from the original message. You can also use basic HTML such as \<br\>, \<b\>, \<i\>, and \<img\> in the message.

|**Type of information**|**Placeholder**|
|:-----|:-----|
|Sender of the message.|%%From%%|
|Recipients listed on the "To" line.|%%To%%|
|Recipients listed on the "Cc" line.|%%Cc%%|
|Subject of the original message.|%%Subject%%|
|Headers from the original message. This is similar to the list of headers in a delivery status notification (DSN) generated for the original message.|%%Headers%%|
|Date the original message was sent.|%%MessageDate%%|

In this example, all messages that contain attachments and are sent to people inside your organization are blocked, and the recipient is notified.

![Rule that notifies recipients when an inbound message is blocked](images/f9a14733-d68a-4528-a736-206325881c47.png)

## Example 3: Modify the subject line for notifications

When a notification is sent to the recipient, the subject line is the subject of the original message. If you want to modify the subject so that it is clearer to the recipient, you must use two transport rules:

- The first rule adds the word "undeliverable" to the beginning of the subject of any messages with attachments.

- The second rule blocks the message and sends a notification message to the sender using the new subject of the original message.

> [!IMPORTANT]
> The two rules must have identical conditions. Rules are processed in order, so the first rule adds the word "undeliverable", and the second rule blocks the message and notifies the recipient.

Here's what the first rule would look like if you want to add "undeliverable" to the subject:

![Rule that prepends Undeliverable to messages with attachments](images/2552b0bd-c69d-48b4-9e69-267fcaf20e70.png)

And the second rule does the blocking and notification (the same rule from Example 2):

![Rule that notifies recipients when an inbound message is blocked](images/f9a14733-d68a-4528-a736-206325881c47.png)

## Example 4: Apply a rule with a time limit

If you have a malware outbreak, you might want to apply a rule with a time limit so that you temporarily block attachments. For example, the following rule has both a start and stop day and time:

![Rule showing a time limit](images/bdc8c4d8-72fa-4c5b-97f2-5fe76d50e643.png)

## See also

[Transport rules in Exchange 2013](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md)
