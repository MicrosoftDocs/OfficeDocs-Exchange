---
localization_priority: Normal
description: When Unified Messaging (UM) users connect to the voice mail system by telephone, they use Outlook Voice Access to navigate the menu system. Before users can access the voice mail system, the system prompts them to enter their PIN. As the administrator, you can configure PIN settings and requirements and perform PIN management tasks. After a user has been enabled for voice mail and a PIN has been generated, the user's PIN is stored encrypted in the user's mailbox.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: ef6d9151-d333-4f52-9338-273f7a291e54
ms.reviewer: 
f1.keywords:
- NOCSH
title: Set Outlook Voice Access PIN security in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Set Outlook Voice Access PIN security in Exchange Online

When Unified Messaging (UM) users connect to the voice mail system by telephone, they use Outlook Voice Access to navigate the menu system. Before users can access the voice mail system, the system prompts them to enter their PIN. As the administrator, you can configure PIN settings and requirements and perform PIN management tasks. After a user has been enabled for voice mail and a PIN has been generated, the user's PIN is stored encrypted in the user's mailbox.

> [!NOTE]
> Outlook Voice Access users must use touchtone (also called dual tone multi-frequency (DTMF)) inputs to enter their PIN to access their UM-enabled mailbox. Speech recognition isn't available for PIN entry.

## PIN overview
<a name="Overview"> </a>

A PIN is a numeric string that's used in certain systems so that a user can be authenticated and gain access to the system. PINs are most frequently used for automatic teller machines (ATMs). They're also used instead of alphanumeric passwords for voice mail systems. The strength of a PIN depends on its length, how well it's protected, and how difficult it is to guess.

 In Unified Messaging, Outlook Voice Access users enter their PIN on an analog, digital, or mobile telephone so that they can access email, voice mail, contact, and calendaring information in their Exchange Server mailbox.

In UM, PIN policies are defined and configured on a UM mailbox policy. You can create multiple UM mailbox policies depending on your requirements. When you enable a user for voice mail, you link the user to an existing UM mailbox policy. The UM PIN policies that are configured on the UM mailbox policy should be based on the security requirements of your organization.

## PIN requirements
<a name="Requirements"> </a>

The following are several PIN configuration settings that you can set on a UM mailbox policy.

### Minimum PIN length

The **Minimum PIN length** setting specifies the minimum number of digits that a mailbox PIN must contain. The range is 4 through 24, and the default is 6. If you enter 0, users aren't required to enter a PIN.

> [!IMPORTANT]
> Configuring this setting with zero isn't a recommended practice. If you configure the setting to zero, you greatly decrease the level of security for your network.

If you change the minimum PIN length to a higher value, current Outlook Voice Access users will be prompted to create a new PIN that contains the new minimum number of digits before they can continue.

> [!NOTE]
> Increasing this number creates a more secure UM environment. However, setting it too high can result in users forgetting their PIN.

### Enforce PIN lifetime

The **Enforce PIN lifetime** setting controls the time interval, in days, from the date Outlook Voice Access users last changed their PIN to the date they'll be forced to change their PIN again. The range is 0 through 999, and the default is 60 days. If 0 is entered, the PIN won't expire.

> [!NOTE]
> Unified Messaging won't notify users when their PIN is about to expire.

### Number of sign-in failures before PIN reset

The **Number of sign-in failures before PIN reset** setting specifies the number of sequential unsuccessful sign-in attempts before the mailbox PIN is automatically reset. To disable this feature, set this setting to unlimited. Otherwise, it must be set to a number lower than the **Number of sign-in failures before lockout** setting. The range is 1 through 998, and the default is 5.

> [!NOTE]
> To increase security for UM-enabled users, enter a number that's less than 5.

### Number of sign-in failures before lockout

The **Number of sign-in failures before lockout** setting specifies how many PIN entry errors in successive calls Outlook Voice Access users can make before they're locked out of their mailbox. By default, after 5 attempts are made, the PIN is automatically reset. The range is 1 through 999, and the default is 15.

> [!NOTE]
> To increase security, decrease the number of failed attempts that are allowed. But remember that decreasing it to a number much lower than the default may result in users being locked out unnecessarily. Unified Messaging will generate warning events that can be viewed using Event Viewer if PIN authentication fails for a UM-enabled user or the user is unsuccessful in trying to sign in to the system.

### Allow common PIN patterns

The **Allow common PIN patterns** setting is used to either enable or disable the use of common number patterns when creating a PIN. By default, this setting is disabled and won't allow Outlook Voice Access users to enter the following number patterns:

- **Sequential numbers**: PIN values that consist completely of consecutive numbers. Examples of sequential numbers for a PIN are 1234 and 65432.

- **Repeated numbers**: PIN values that consist of repeated numbers. Examples of repeated numbers are 11111 and 22222.

- **Suffix of mailbox extension**: PIN values that consist of the suffix of a user's mailbox extension. If the mailbox extension is 36697, the PIN can't be 6697.

### PIN recycle count

The **PIN recycle count** setting configures the number of different PINs a user must use before any PINs that were previously used can be reused. The range is 1 through 20, and the default is 5.

## Managing Outlook Voice Access PINs
<a name="Managing"> </a>

When planning for Outlook Voice Access PINs, you must choose the appropriate levels of security for your organization. You must carefully consider the Outlook Voice Access PIN requirements and how your PIN security settings meet or exceed your organization's security policy.

> [!IMPORTANT]
> It's a security best practice to implement strong PIN requirements for Outlook Voice Access users. This can be enforced by creating UM mailbox policy PIN policies that require six or more digits for PINs, which increases the level of security for your network.

After you set the Outlook Voice Access PIN requirements, you must create and configure a UM mailbox policy to enforce your organizational PIN requirements. For details about how to create a UM mailbox policy, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md). For details about how to manage UM mailbox policies, see [Manage a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/manage-um-mailbox-policy.md).

> [!NOTE]
> After you create the UM mailbox policy, you must link the UM-enabled user or users with the appropriate UM mailbox policy. You can do this by using the **Enable-UMMailbox** cmdlet in Exchange Online PowerShell or by using the Exchange admin center (EAC). For more information about Exchange Online PowerShell cmdlet, see [Enable-UMMailbox](https://docs.microsoft.com/powershell/module/exchange/enable-ummailbox).

There are situations in which Outlook Voice Access users forget their PIN or are locked out of voice mail access to their mailbox. In either case, it may be necessary for you to reset a UM-enabled user's PIN. For details, see [Reset a voice mail PIN](reset-a-voice-mail-pin.md).

You can retrieve PIN information for a user who is enabled for Unified Messaging. The information returned to you is calculated by using the encrypted PIN data stored in the user's mailbox. This lets you view PIN information for the user and also indicates whether the user has been locked out of their mailbox. For details, see [Retrieve voice mail PIN information](retrieve-voice-mail-pin-information.md).
