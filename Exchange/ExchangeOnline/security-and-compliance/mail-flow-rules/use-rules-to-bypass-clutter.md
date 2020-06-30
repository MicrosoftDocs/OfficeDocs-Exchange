---
localization_priority: Normal
description: Learn how to create mail flow rules so messages aren't placed in the Clutter folder in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 58e413f0-aa27-4307-bffd-4df03090a15e
ms.reviewer: 
f1.keywords:
- NOCSH
title: Use mail flow rules so messages can bypass Clutter in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Use mail flow rules so messages can bypass Clutter in Exchange Online

If you want to be sure that you receive particular messages, you can create a mail flow rule (also known as a transport rule) that makes sure that these messages bypass your Clutter folder. Check out [Use Clutter to sort low-priority messages in Outlook](https://support.microsoft.com/office/7b50c5db-7704-4e55-8a1b-dfc7bf1eafa0) for more info on Clutter.

For additional management tasks related to mail flow rules, check out [Mail flow rules (transport rules) in Exchange Online](mail-flow-rules.md) and the [New-TransportRule](https://docs.microsoft.com/powershell/module/exchange/new-transportrule) PowerShell topic. If you're new to Exchange Online PowerShell, check out [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail flow" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For more information about opening and using the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

- To learn how to connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?linkid=396554).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

## Use the Exchange admin center to create a mail flow rule to bypass the clutter folder

This example allows all messages with title "Meeting" to bypass clutter.

1. In the Exchange admin center (EAC), go to **Mail flow** \> **Rules**. Click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) and then choose **Create a new rule...**.

   ![Art example: If subject contains meeting, bypass clutter](../../media/75957aa4-4b2a-4142-92ff-07f8ccc64d82.png)

2. After you're done creating the new rule, click **Save** to start the rule.

## Use Exchange Online PowerShell to create a mail flow rule to bypass the clutter folder

This example allows all messages with title "Meeting" to bypass clutter.

```PowerShell
New-TransportRule -Name "<Unique rule name>" -SubjectContainsWords "Meeting" -SetHeaderName "X-MS-Exchange-Organization-BypassClutter" -SetHeaderValue "true"
```

> [!IMPORTANT]
> In this example, both `X-MS-Exchange-Organization-BypassClutter` and `true` are case sensitive.

For detailed syntax and parameter information, see [New-TransportRule](https://docs.microsoft.com/powershell/module/exchange/new-transportrule).

## How do you know this worked?

You can check email message headers to see if the email messages are landing in the Inbox due to the Clutter mail flow rule bypass. Pick an email message from a mailbox in your organization that has the Clutter bypass mail flow rule applied. Look at the headers stamped on the message, and you should see the **X-MS-Exchange-Organization-BypassClutter: true** header. This means the bypass is working. Check out the [View the internet header information for an email message](https://support.microsoft.com/office/cd039382-dc6e-4264-ac74-c048563d212c) topic for info on how to find the header information.

> [!NOTE]
> Calendar items (accepted, sent, or declined meetings notifications) won't contain this header.
