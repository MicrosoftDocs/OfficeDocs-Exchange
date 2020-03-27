---
localization_priority: Normal
description: Admins can learn about remote domains (message formatting settings for external domains) in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f191e052-658d-4c74-bfe7-bcb1d525e4e3
ms.reviewer: 
title: Remote domains in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
f1.keywords:
- NOCSH
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Remote domains in Exchange Online

There are many reasons why you might want to control the types and the format of messages that your users send from Exchange Online to recipients in external domains. For example:

- You don't want to let your users forward messages to recipients in other domains.

- You work with an organization that you don't want to receive automatic messages from (for example, non-delivery reports and out-of-office replies).

- You have a business partner that's outside your organization, and you'd like that partner to receive the same out-of-office replies as those received by people inside your organization.

- Your users frequently send email to a company that supports limited email formats, and you'd like to make sure all emails sent to that organization are sent in a format that they can read.

To accomplish this, you use what's called a _remote domain_. The remote domain settings override settings that your users might configure in Outlook or Outlook on the web (formerly known as Outlook Web App), or that you configure in the Exchange admin center (EAC) or Exchange Online PowerShell. For example, users might have an out-of-office reply set up for people outside the organization, but if a sender from a remote domain sends mail to them, and the remote domain is not set to receive out-of-office replies, no out-of-office reply is sent. To change the settings, you can:

- Create a remote domain for a specific domain, and set unique properties for emails sent to that domain.

- Modify the settings for the default remote domain. If you have no other remote domains set up, changes to the default remote domain apply to all external domains. If you have other remote domains set up, changes to the default remote domain apply to all other external domains.

For instructions on how to create and configure remote domains, see [Manage remote domains in Exchange Online](manage-remote-domains.md).

## Reducing or increasing information flow to another company

When a message comes from outside your organization, there are several types of replies that are automatically generated. Some types of replies are set up by users in Outlook or Outlook on the web, and others are set up by admins. Because the remote domain settings override settings configured by users, as well as mail user and mail contact settings configured by admins, you can choose which types of automatic replies are sent to everyone on a remote domain.

If a remote domain configuration blocks a specific type of reply, like a non-delivery report, from being sent to recipients in that domain, the reply is generated, but then it is deleted before it is sent. No error message is sent. For example, if you turn off automatic forwarding on the default remote domain, when users try to automatically forward email to another domain, they can change their settings or create the Inbox rule, but their messages won't be forwarded.

The following table shows the types of replies you can control in a remote domain and the settings that each remote domain setting overrides.

|**Type of reply**|**Description**|**Per-user settings that this remote domain setting overrides**|
|:-----|:-----|:-----|
|Out-of-office messages|Specify whether an out-of-office message should be sent to people on the remote domain, and if so, which message to use. You can select either the reply that the user on your domain set up for people outside your organization, or the one for people inside your organization. The default is to send the out-of-office reply for people outside your organization.|This setting overrides out-of-office reply settings specified by individual users in [Outlook](https://go.microsoft.com/fwlink/p/?LinkId=398992) or [Outlook on the web](https://go.microsoft.com/fwlink/p/?LinkId=398993).|
|Automatic replies|Allow or prevent automatic replies to senders on the remote domain. The default is to allow automatic replies.|This setting overrides automatic replies set up by admins using the [Set-MailboxAutoReplyConfiguration](https://docs.microsoft.com/powershell/module/exchange/mailboxes/set-mailboxautoreplyconfiguration) cmdlet.|
|Automatic forwards|Allow or prevent automatically forwarded messages to be sent to people on the remote domain. The default is to allow automatic forwarding.| When users configure automatic forwarding to recipients on a remote domain, the remote domain settings override users' automatic forwarding settings (messages are blocked if automatic forwards are disabled for the remote domain). Users can configure automatic forwarding by using these methods: <br/>• Inbox rules in Outlook or Outlook on the web to forward messages. Learn more about Inbox rules in [Outlook](https://go.microsoft.com/fwlink/p/?LinkId=398878) and [Outlook on the web](https://go.microsoft.com/fwlink/p/?LinkId=398877). <br/>• Forwarding options in Outlook on the web. For more information, see [Forward email from Office 365 to another email account](https://go.microsoft.com/fwlink/p/?linkid=866249). <br/> **Note**: When admins use other methods to configure automatic forwarding for users, the forwarded messages will be affected by the remote domain settings (messages will not get forwarded to recipients on the remote domain if automatic forwards are disabled for the remote domain).
|Delivery reports|Allow or prevent a delivery receipt to be sent to people on the remote domain. The default is to allow sending delivery reports.|An email sender on the remote domain can request a delivery receipt on a message. This remote domain setting can override the sender's request for a delivery receipt and prevent the delivery receipt from being sent. For more information about requesting a delivery receipt, see [Add tracking to email messages](https://go.microsoft.com/fwlink/p/?LinkId=398873).|
|Non-delivery report|Allow or prevent non-delivery reports (also known a NDRs or bounce messages) to be sent to people on the remote domain. The default is to allow sending non-delivery reports.|This remote domain setting is the only way to prevent non-delivery reports from being sent when a message can't be delivered.|
|Meeting forward notifications|Prevent or allow meeting forward notifications to be sent to people on the remote domain. The default is to prevent sending meeting forward notifications.|Meeting forward notifications are automatically created and sent to the meeting organizer when a meeting participant forwards a meeting. Typically, they are sent to meeting organizers only on domains that are part of your Exchange Online organization. Admins can enable them to be sent to meeting organizers on the remote domain.|

## Specifying message format

To make sure that email sent from your Exchange Online organization is compatible with the receiving messaging system in the remote domain, you can specify the message format and character set to use for all email messages sent to that remote domain. For example, if you know that the remote domain is not using Exchange, you can specify to never use Rich Text Format (RTF). The following table describes the message format settings.

|**Setting**|**Description**|**Settings that this overrides**|
|:-----|:-----|:-----|
|Rich Text Format (RTF)| Choose how to format messages: <br/>• **Always**: Use this value if the remote domain uses Exchange. <br/>• **Never**: If the remote domain does not use Exchange, use this value. <br/>• **Follow user settings**: Use message format settings defined by the user. Use this value if you don't know what email system the remote domain uses. <br/> The default is to follow the user's settings.|Message format can be defined in several places: Outlook or Outlook on the web, and the admin can also use the [Set-MailContact](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/set-mailcontact) or [Set-MailUser](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/set-mailuser) cmdlets to modify settings per recipient. <br/> Remote domain settings override settings specified by a user or by the admin. For more information about the message formats and the order of precedence of message format settings, see [Message format and transmission in Exchange Online](../../mail-flow-best-practices/message-format-and-transmission.md).|
|MIME character set and Non-MIME character set|• **None**: Use the character set specified in the message. <br/>• **Select a character set from the list**: If the message does not have a character set, the selected character set is used. <br/> By default, no character sets are specified.|These settings are used only if the message doesn't include a character set. For a complete list of supported character sets, see [Supported character sets for remote domains](supported-character-sets.md).|

If you specify a particular message format for the remote domain, the format of the headers and message content sent to the domain are modified.

## Other settings

You can configure other message settings for remote domains by using Exchange Online PowerShell. For a complete list of settings, see [Set-RemoteDomain](https://docs.microsoft.com/powershell/module/exchange/mail-flow/set-remotedomain).

## More information

- You can't remove the default remote domain.

- You can specify all subdomains when you create a remote domain.

## See also

[Manage remote domains in Exchange Online](manage-remote-domains.md)
