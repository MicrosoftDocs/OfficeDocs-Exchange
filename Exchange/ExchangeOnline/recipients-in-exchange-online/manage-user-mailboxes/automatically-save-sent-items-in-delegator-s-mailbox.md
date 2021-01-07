---
localization_priority: Normal
description: Set up mailboxes so messages sent by a delegate are stored in both the delegate and delegator's Sent Items.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: f15297f3-32c3-44b6-87b5-dd64dc2bcf7e
ms.reviewer: 
f1.keywords:
- NOCSH
title: Automatically save sent items in delegator's mailbox
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- BCS160
- MET150
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Automatically save sent items in delegator's mailbox

Mailboxes in Microsoft 365 or Office 365 can be set up so that someone (such as an executive assistant) can access the mailbox of another person (such as a manager) and send mail as them. These people are often called the delegate and the delegator, respectively. We'll call them "assistant" and "manager" for simplicity's sake. When an assistant is granted access to a manager's mailbox, it's called delegated access.

People often set up delegated access and send permissions to allow an assistant to manage a manager's calendar where they need to send and respond to meeting requests. By default, when an assistant sends mail as, or on behalf of, a manager, the sent message is stored in the assistant's Sent Items folder. You can use this article to change this behavior so that the sent message is stored in both the assistant and manager's Sent Items folders.

Let's take a look at a quick example of how this would work in real life:

- Mary is the Vice President of Global Sales. She has an extremely busy schedule and has Rob, her executive assistant, to help manage her calendar.

- To help Mary, Rob's been granted delegated access to Mary's mailbox and to send messages on her behalf. This allows him to see what's on her calendar; schedule, accept, and decline meeting requests; and respond to messages.

- Messages that Rob sends on behalf of Mary are stored in his Sent Items folder. Mary wants a copy so Rob manually copies messages he's sent on her behalf from his Sent Items folder to her Sent Item folder.

- Rob's wonders if there's a better way to handle Sent Items so he asks his IT Help Desk. He learns Mary's mailbox can be set up to store messages he sends on her behalf in both his Sent Items and her Sent Items automatically. This is exactly what he wants so he asks the Help Desk to set it up.

## Send As...Send on behalf...what do they mean and which should I choose?

When you set up someone as a delegate on a manager's mailbox, you can choose whether they "Send as" the manager, or "Send on behalf" of them. The difference is subtle, but can be important in some organizations:

- **Send As** When someone has "Send as" permissions on a mailbox, messages they send from that mailbox will show only the mailbox owner's name in the From: field of the message. In the example above, if Rob has "Send as" permissions on Mary's mailbox, messages he sends from her mailbox will show **From: Mary** to recipients.

- **Send on behalf** When someone has "Send on behalf" permissions on a mailbox, messages they send from the owner's mailbox will show that the message was sent by someone on behalf of the mailbox owner. In the example above, if Rob has "Send on behalf" permissions on Mary's mailbox, messages he sends from her mailbox will show **From: Rob on behalf of Mary** to recipients.

The send permissions that someone has on another user's mailbox are important when thinking about how sent items should be handled. This is because you can decide, for each level of permissions, whether messages should be stored in just the assistant's Sent Items folder or in both the assistant and manager's Sent Items folders. Microsoft 365 and Office 365 default to storing sent items for messages sent with "Send as" and "Send on behalf" permissions in the assistant's Sent Items only. You can change that default behavior using the steps below.

> [!TIP]
> Managers might have multiple assistants with different levels of permissions. In the example above, while Rob may be able to send messages on behalf of Mary, she could have another assistant that can Send as Mary. If this was the case, Mary's IT department could do the steps for both "Send as" and "Send on behalf" permissions.

## Use the new EAC to automatically save sent items in the delegator's mailbox

1. In the new EAC, navigate to **Recipients > Mailboxes**.
   
2. In the list of user mailboxes, click the mailbox that you want to configure the sent items to be saved for the delegator. A display pane is shown for the selected user mailbox.
   
3. Under **Mailbox settings**, click **Manage mailbox delegation**.

4. In the **Manage mailbox delegation** settings, do the following.
    1. In the **Send on behalf** row, click **Edit**.
    1. Click **Add permissions**.
    1. Check the checkbox of the person whom you want to configure as the delegate.
  
 5. Click **Save** to save your change. A message Manage mailbox delegation settings updated successfully is displayed. Click **Close** to exit.

## How do I set up a mailbox to save messages "Sent as" a manager when they're sent by an assistant?

When you do these steps, any messages **sent as** the manager whose mailbox you're configuring, will be saved to the manager's Sent Items folder. To set this up, just follow the steps below. You'll need to use Windows PowerShell to complete the steps; if you haven't used it before, go to [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/exchange-online-powershell) for instructions on how to get connected. There's a great video too!

1. [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/exchange-online-powershell).

2. Get the email address of the manager.

3. Use the following syntax in Exchange Online PowerShell window:

   ```PowerShell
   Set-Mailbox <manager's email address> -MessageCopyForSentAsEnabled $true
   ```

For example, if Mary's email address is mary@contoso.com, her IT department would run the following command:

```PowerShell
Set-Mailbox mary@contoso.com -MessageCopyForSentAsEnabled $true
```

That's it! The manager will now automatically get a copy of any messages sent by an assistant, in their Sent Items folder.

> [!TIP]
> You can turn this off by going through the steps above and replacing **$true** with **$false** in the **[Set-Mailbox]** command. For example, to turn it off for Mary, they'd run the command: `Set-Mailbox -Identity mary@contoso.com -MessageCopyForSentAsEnabled $false`.

## How do I set up a mailbox to save messages "Sent on behalf" of a manager when they're sent by an assistant?

When you do these steps, any messages **sent on behalf** the manager whose mailbox you're configuring, will be saved to the manager's Sent Items folder. To set this up, just follow the steps below. You'll need to use Windows PowerShell to complete the steps; if you haven't used it before, go to [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/exchange-online-powershell) for instructions on how to get connected. There's a great video too!

1. [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/exchange-online-powershell).

2. Get the email address of the manager.

3. Use the following syntax in the Exchange Online PowerShell:

   ```PowerShell
   Set-Mailbox <manager's email address> -MessageCopyForSendOnBehalfEnabled $true
   ```

For example, if Mary's email address is mary@contoso.com, her IT department would run the following command

```PowerShell
Set-Mailbox mary@contoso.com -MessageCopyForSendOnBehalfEnabled $true
```

That's it! The manager will now automatically get a copy of any messages sent by an assistant, in their Sent Items folder.

> [!TIP]
> You can turn this off by going through the steps above and replacing **$true** with **$false** in the **[Set-Mailbox]** command. For example, to turn it off for Mary, they'd run the command  `Set-Mailbox mary@contoso.com -MessageCopyForSendOnBehalfEnabled $false`.
