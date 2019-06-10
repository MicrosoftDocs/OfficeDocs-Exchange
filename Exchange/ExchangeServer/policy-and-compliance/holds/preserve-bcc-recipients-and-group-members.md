---
localization_priority: Normal
description: In-Place Hold and Litigation Hold allow you to preserve mailbox content to meet regulatory compliance and eDiscovery requirements. Information about recipients directly addressed in the To and Cc fields of a message is included in all messages by default, but your organization may require the ability to search for and reproduce details about all recipients of a message.
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: eb8ddf15-0080-457e-9d83-e73e193da334
ms.date: 7/6/2018
ms.reviewer: 
title: Preserve Bcc and expanded distribution group recipients for eDiscovery
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Preserve Bcc and expanded distribution group recipients for eDiscovery

In-Place Hold and Litigation Hold allow you to preserve mailbox content to meet regulatory compliance and eDiscovery requirements. Information about recipients directly addressed in the To and Cc fields of a message is included in all messages by default, but your organization may require the ability to search for and reproduce details about all recipients of a message. This includes

- **Recipients addressed using the Bcc field of a message**: Bcc recipients are stored in the message in the sender's mailbox, but not included in headers of the message delivered to recipients.

- **Expanded distribution group recipients**: Recipients who receive the message because they're members of a distribution group to which the message was addressed, either in the To, Cc or Bcc fields.

Exchange 2016 and Exchange 2019 preserve information about Bcc and expanded distribution group recipients. You can search for this information by using an In-Place eDiscovery search .

## How Bcc recipients and expanded distribution group recipients are preserved

As stated earlier, information about Bcc'ed recipients is stored with the message in the sender's mailbox. This information is indexed and available to In-Place eDiscovery and Hold.

Information about expanded distribution group recipients is stored with the message after you place a mailbox on In-Place Hold or Litigation Hold. Distribution group membership is determined at the time the message is sent. The expanded recipients list stored with the message is not impacted by changes to membership of the group after the message is sent.

|**Information about...**|**Is stored in...**|**Is stored by default?**|**Is accessible to...**|
|:-----|:-----|:-----|:-----|
|To and Cc recipients|Message properties in the sender and recipients' mailboxes.|Yes|Sender, recipients, and compliance officers|
|Bcc recipients|Message property in the sender's mailbox.|Yes|Sender and compliance officers|
|Expanded distribution group recipients|Message properties in the sender's mailbox.|No. Expanded distribution group recipient information is stored after a mailbox is placed on In-Place Hold or Litigation Hold.|Compliance officers|

## Searching for messages sent to Bcc and expanded distribution group recipients

When searching for messages sent to a recipient, eDiscovery search results now include messages sent to a distribution group that the recipient is a member of. The following table shows the scenarios where messages sent to Bcc and expanded distribution group recipients are returned in eDiscovery searches.

Scenario 1: John is a member of the US-Sales distribution group. This table shows eDiscovery search results when Bob sends a message to John directly or indirectly via a distribution group.

|**When you search Bob's mailbox for messages sent...**|**And the message is sent with...**|**Results include message?**|
|:-----|:-----|:-----|
|To:John|John on TO|Yes|
|To:John|US-Sales on TO|Yes|
|To:US-Sales|US-Sales on TO|Yes|
|Cc:John|John on CC|Yes|
|Cc:John|US-Sales on CC|Yes|
|Cc:US-Sales|US-Sales on CC|Yes|

Scenario 2: Bob sends an email to John (To/Cc) and Jack (Bcc directly, or indirectly via a distribution group). The table below shows eDiscovery search results.

|**When you search...**|**For messages sent...**|**Results include message?**|**Notes**|
|:-----|:-----|:-----|:-----|
|Bob's mailbox|To/Cc:John|Yes|Presents an indication that Jack was Bcc'ed.|
|Bob's mailbox|Bcc:Jack|Yes|Presents an indication that Jack was Bcc'ed.|
|Bob's mailbox|Bcc:Jack (via distribution group)|Yes|List of members of the Bcc'ed distribution group, expanded when the message was sent, is visible in eDiscovery search preview, export and logs.|
|John's mailbox|To/Cc:John|Yes|No indication of Bcc recipients.|
|John's mailbox|Bcc:Jack (directly or via distribution group)|No|Bcc information is not stored in the message delivered to recipients. You must search the sender's mailbox.|
|Jack's mailbox|To/Cc:John (directly or via distribution group)|Yes|To/Cc information is included in message delivered to all recipients.|
|Jack's mailbox|Bcc:Jack (directly or via distribution group)|No|Bcc information is not stored in the message delivered to recipients. You must search the sender's mailbox.|

## Frequently asked questions

 **Q. When and where is Bcc recipient information stored?**

A. Bcc recipient information is preserved by default in the original message in sender's mailbox. If the Bcc recipient is a distribution group, distribution group membership is only expanded if the sender's mailbox is on hold.

 **Q. When and where is the list of expanded distribution group recipients stored?**

A. Group membership is expanded at the time the message is sent. The list of expanded distribution group members is stored in the original message in the sender's mailbox. The sender's mailbox must be on In-Place Hold or Litigation Hold.

 **Q. Can the To/Cc recipients see which recipients were Bcc'ed?**

A. No. This information is not included in message headers, and isn't visible to To/Cc recipients. The sender can see the Bcc field stored in the original message stored in their mailbox. Compliance officers can see this information when searching the sender's mailbox.

 **Q. How can I ensure expanded distribution group recipients are always preserved?**

A. To ensure expanded distribution group members are always preserved with a message, [Place all mailboxes on hold](place-all-mailboxes-on-hold.md).

 **Q. Which types of groups are supported?**

A. Distribution groups, mail-enabled security groups, and dynamic distribution groups are supported.

 **Q. Is there a limit on the number of distribution group recipients that are expanded and stored in the message?**

A. Up to 10,000 members of a distribution group is preserved.

 **Q. Are nested distribution groups supported?**

A. Yes, 25 levels of nested distribution groups are expanded.

 **Q. Where is the Bcc and expanded distribution group recipient information visible?**

A. Bcc and expanded distribution group recipients information is visible to Compliance officers when performing an eDiscovery search. Bcc and expanded distribution group recipients are included in search results copied to a Discovery mailbox or exported to a PST file and in the eDiscovery log included in search results. Bcc recipient information is also available in search preview.

 **Q. What happens if a member of a distribution group is hidden from the organization's global address list (GAL)?**

A. There's no impact. If recipients are hidden from the GAL, they're still included in the list of recipients for the expanded distribution group.
