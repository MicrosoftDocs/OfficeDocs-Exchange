---
title: 'Enable PIN-less sign-Ins for voice mail: Exchange 2013 Help'
TOCTitle: Enable PIN-less sign-Ins for voice mail
ms:assetid: 54133753-317c-42ef-9b0d-ca9f2d2d6bd7
ms:mtpsurl: https://technet.microsoft.com/library/Gg602127(v=EXCHG.150)
ms:contentKeyID: 49315417
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable PIN-less sign-Ins for voice mail in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can set up Unified Messaging (UM) so that your users can sign in to their voice mail without using a PIN. By default, Outlook Voice Access users are prompted to enter a PIN to sign in to their mailbox and access their voice mail, email, calendar, personal contacts, the directory, and personal options.

> [!WARNING]
> Enabling PIN-less sign-ins for a single user or a group of users that are enabled for voice mail decreases the level of security for voice mail and poses a security risk to your organization.

To enable PIN-less sign-ins, you must set the parameter *AllowPinlessVoiceMailAccess* to `$true` on the UM mailbox policy and set the parameter *PinlessAccessToVoiceMailEnabled* to `$true` on the UM mailbox. By default, both parameters are set to `$false`, which requires an Outlook Voice Access user to enter their PIN when they access their voice mail.

Setting both parameters to `$true` allows you to enable PIN-less sign-ins to voice mail for a large group of users who are associated with a UM mailbox and also enable PIN-less sign-ins for a single UM mailbox or a subset of UM mailboxes. Even if you enable PIN-less sign-ins to voice mail for a group of UM-enabled users or a single UM-enabled user, when they access their email, calendar, personal contacts, the directory, or personal options, they'll be prompted to enter their PIN.

To enable PIN-less sign-ins to voice mail for a user, the following conditions must be met:

- You've run the following cmdlet on the UM mailbox policy: `Set-UMMailboxPolicy -id MyUMMailboxPolicy -AllowPinlessVoiceMailAccess $true`

- You've run the following cmdlet on the mailbox of the UM-enabled user: `Set-UMMailbox -id tonys@contoso.com -PinlessAccessToVoiceMailEnabled $true`

- The UM-enabled user is associated with the same UM mailbox policy for which you enabled PIN-less sign-ins.

- The UM-enabled user dials in to Outlook Voice Access from a phone number that's been assigned to them.

- You can only use the Shell to perform this procedure. To learn how to open the Shell in your on-premises Exchange organization, see [Open the Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell). To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online using remote PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

For additional tasks related to UM mailbox policies, see [UM mailbox policy procedures](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policy-procedures).

For additional tasks related to UM mailboxes, see [Voice mail-enabled user procedures](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/voice-mail-enabled-user-procedures).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy).

- Before you perform these procedures, confirm that the user or users have been enabled for UM and voice mail. For detailed steps, see [Enable a user for voice mail](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail).

## Use the Shell to enable PIN-less access to voice mail for UM-enabled users on a UM mailbox policy

This example enables PIN-less voice mail access on a UM mailbox policy named `MyUMMailboxPolicy` for users associated with the mailbox policy who dial in to Outlook Voice Access.

```powershell
Set-UMMailboxPolicy -id MyUMMailboxPolicy -AllowPinlessVoiceMailAccess $true
```

## Use the Shell to enable PIN-less access to voice mail on a UM-enabled user's mailbox

This example enables PIN-less voice mail access for the user who dials in to Outlook Voice Access to reach the mailbox named `tonys@contoso.com`.

```powershell
Set-UMMailbox -id tonys@contoso.com -PinlessAccessToVoiceMailEnabled $true
```
