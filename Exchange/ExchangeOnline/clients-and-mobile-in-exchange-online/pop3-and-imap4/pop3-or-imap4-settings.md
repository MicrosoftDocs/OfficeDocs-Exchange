---
localization_priority: Normal
description: You use the Set-CASMailbox cmdlet to configure the PO3 and IMAP4 options for each user. The configuration options are described in the following table.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: bf4ca453-e79c-4b87-a9a5-3ae1b21181e2
ms.reviewer: 
title: Set POP3 or IMAP4 settings for a user
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Set POP3 or IMAP4 settings for a user

You use the **Set-CASMailbox** cmdlet to configure the PO3 and IMAP4 options for each user. The configuration options are described in the following table.

|**Parameter**|**Description**|**Values**|
|:-----|:-----|:-----|
|_PopForceICalForCalendarRetrievalOption_ <br/> _ImapForceICalForCalendarRetrievalOption_|Sets the preferred format for meeting requests. <br/> By default, meeting requests appear as Outlook on the web (formerly known as Outlook Web App) links. You can change them to iCal format.|`$true`: Meeting requests are all Outlook on the web links  <br/> `$false`: Meeting requests are all iCal format|
|_PopSuppressReadReceipt_ <br/> _ImapSuppressReadReceipt_|Sets whether to send read receipts when a message is downloaded and again when it is opened or just when the message is opened  <br/> By default, if a read receipt is requested, two read receipts are sent: one when a user downloads a message and another when the user opens the message. You can change it so that only one read receipt is sent: when the user opens the message.|`$false`: POP3 or IMAP4 users are sent a read receipt each time a recipient downloads a message. Users are also sent a read receipt when the user opens the message. This is the default setting. <br/> `$true`: POP3 or IMAP4 users that use the **send read receipt for messages I send** option in their email client programs receive a read receipt only when the recipient opens the message.|
|_PopMessagesRetrievalMimeFormat_ <br/> _ImapMessagesRetrievalMimeFormat_|Sets the preferred format for received messages. <br/> The default is to use the best format based on the message.|Use a numeral or a text value. <br/> `0` or `TextOnly`: Text only  <br/> `1` or `HtmlOnly`: HTML  <br/> `2` or `HtmlAndTextAlternative`: HTML and alternative text  <br/> `3` or `TextEnriched`: Enriched text  <br/> `4` or `TextEnrichedAndTextAlternative`: Enriched text and alternative text  <br/> `5` or `BestBodyFormat`: Best body format. This is the default value. <br/> `6` or `Tnef`: Transport-Neutral Encapsulation Format (TNEF). Also known as rich text format, Outlook rich text format, or MAPI rich text format.|
|_PopEnableExactRFC822Size_ <br/> _ImapEnableExactRFC822Size_|Sets whether to calculate the exact size of messages. <br/> Changing this value is not recommended unless the default value causes problems for your email client. By default, the estimated message size, rather than the exact message size, is sent to the email client.|`$true`: Use actual message size. <br/> `$false`: Use estimated message size. This is the default.|

For additional information related to POP3 and IMAP4, see [POP3 and IMAP4](pop3-and-imap4.md).

## What do you need to know before you begin?

- Estimated time to finish each procedure: five minutes.

- You can only use Exchange Online PowerShell to perform this procedure. To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?linkid=396554).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "POP3 and IMAP4 settings" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to set the meeting request format for a POP3 or IMAP4 user

The following example sets all meeting requests in incoming mail to USER01 to iCal format for a POP3 user.

```PowerShell
Set-CASMailbox USER01 -PopUseProtocolDefaults $false -PopForceICalForCalendarRetrievalOption $true
```

The following example sets all meeting requests in incoming mail to USER01 to iCal format for an IMAP4 user.

```PowerShell
Set-CASMailbox USER01-ImapUseProtocolDefaults $false -ImapForceICalForCalendarRetrievalOption $true
```

### How do you know this worked?

To verify that you successfully set the meeting request format for a POP3 or an IMAP4 user, run the following command in Exchange Online PowerShell and verify that the values displayed are the values that you configured:

```PowerShell
Get-CASMailbox USER01 | format-list *ForceIcal*,*UseProtocolDefaults
```

## Use Exchange Online PowerShell to set the suppress read receipt option for a POP3 or IMAP4 user

The following example sets it up so that the POP3 sender receives a read receipt only when the message is opened.

```PowerShell
Set-CASMailbox USER01 -PopUseProtocolDefaults $false -PopSuppressReadReceipt $true
```

The following example sets it up so that the IMAP4 sender receives a read receipt only when the message is opened.

```PowerShell
Set-CASMailbox USER01 -ImapUseProtocolDefaults $false -ImapSuppressReadReceipt $true
```

### How do you know this worked?

To verify that you successfully set the read receipt option for a POP3 or an IMAP4 user, run the following command in Exchange Online PowerShell and verify that the values displayed are the values that you configured:

```PowerShell
Get-CASMailbox USER01 | format-list *SuppressReadReceipt,*UseProtocolDefaults
```

## Use Exchange Online PowerShell to set the message retrieval format for a POP3 or IMAP4 user

The following example sets the message retrieval format to text only for POP3 access for `USER01`.

```PowerShell
Set-CASMailbox USER01 -PopUseProtocolDefaults $false -PopMessagesRetrievalMimeFormat TextOnly
```

The following example sets the message retrieval format to text only for IMAP4 access for `USER01`.

```PowerShell
Set-CASMailbox USER01 -ImapUseProtocolDefaults $false -ImapMessagesRetrievalMimeFormat TextOnly
```

### How do you know this worked?

To verify that you successfully set the message retrieval format for a POP3 or an IMAP4 user, run the following command in Exchange Online PowerShell and verify that the values displayed are the values that you configured:

```PowerShell
Get-CASMailbox USER01 | format-list *MessagesRetrievalMimeFormat,*UseProtocolDefaults
```

## Use Exchange Online PowerShell to set the message size calculation for a POP3 or IMAP4 user

This example calculates the exact size of POP messages for USER01.

> [!IMPORTANT]
> Set the _PopEnableExactRFC822Size_ parameter to `$true` only if the POP client doesn't work for this user.

```PowerShell
Set-CASMailbox USER01 -PopUseProtocolDefaults $false -PopEnableExactRFC822Size $true
```

This example calculates the exact size of IMAP messages for USER01.

> [!IMPORTANT]
> Set the _ImapEnableExactRFC822Size_ parameter to `$true` only if the IMAP client doesn't work for this user.

```PowerShell
Set-CASMailbox USER01 -ImapUseProtocolDefaults $false -ImapEnableExactRFC822Size $true
```

### How do you know this worked?

To verify that you successfully set the message size calculation for a POP3 or IMAP4 user, run the following command in Exchange Online PowerShell and verify that the values displayed are the values that you configured::

```PowerShell
Get-CASMailbox USER01 | format-list *EnableExact*,*UseProtocolDefaults
```

## For more information

[Connect to Exchange Online Using Remote PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell)

[POP3 and IMAP4](pop3-and-imap4.md)

[Enable or Disable POP3 or IMAP4 access for a user](enable-or-disable-pop3-or-imap4-access.md)

[Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox)
