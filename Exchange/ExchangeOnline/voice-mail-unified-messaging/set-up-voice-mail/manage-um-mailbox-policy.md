---
localization_priority: Normal
description: After you create a Unified Messaging (UM) mailbox policy, you can view and configure a variety of settings. For example, you can configure UM features like Voice Mail Preview or Play on Phone and other security-related options such as Protected Voice Mail and PIN policy settings.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 704b001c-3e6f-4ed5-bbe5-42a778f6ac0d
ms.reviewer: 
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Servers.UnifiedMessaging.UMMailboxPolicyGeneralTab
title: Manage a UM mailbox policy in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage a UM mailbox policy in Exchange Online

After you create a Unified Messaging (UM) mailbox policy, you can view and configure a variety of settings. For example, you can configure UM features like Voice Mail Preview or Play on Phone and other security-related options such as Protected Voice Mail and PIN policy settings.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to manage a UM mailbox policy

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM dial plan** page, under **UM Mailbox Policies**, on the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

   - Use **General** to view and configure settings for a UM mailbox policy. For example, you can view the dial plans associated with the UM mailbox policy or disable missed call notifications for users who are associated with a specific UM mailbox policy. When you modify the settings on a UM mailbox policy, the settings are applied to all users who are associated with the UM mailbox policy. You can view or configure the following:

   - **UM dial plan**: Displays the name of the dial plan associated with the UM mailbox policy. This is the name of the dial plan displayed in Exchange Online PowerShell.

     When a new UM mailbox policy is created, it must be associated with a dial plan. After the UM mailbox policy is created and associated with a dial plan, the settings defined on the mailbox policy are applied to the users who are associated with the dial plan. By default, when you create a UM dial plan using Exchange Online PowerShell, it will also create a UM mailbox policy.

   - **Name**: Type the name of the dial plan. A UM dial plan name is required and must be unique. However, it's used only for display in the EAC and Exchange Online PowerShell. If you have to change the display name of the dial plan after it's been created, you must first delete the existing UM dial plan and then create another dial plan that has the appropriate name. If your organization uses multiple UM dial plans, we recommend that you use meaningful names for your UM dial plans. The maximum length of a UM dial plan name is 64 characters, and it can include spaces. (If you're integrating with Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server it's not recommended that you use spaces.) However, it can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

   - **Limit on personal greetings (minutes)**: Use this text box to enter the maximum number of minutes that users who are associated with the UM mailbox policy can use when they record their voice mail greeting. You can modify this setting after the UM mailbox policy is created. Only numeric characters are allowed. The valid range for the greeting is from 1 through 10 minutes. The default setting is 5 minutes.

   - **Allow voice mail preview**: Select or clear this check box to enable or disable the Voice Mail Preview feature for users associated with the UM mailbox policy. Enabling this setting allows users to receive the text of a voice mail message in the message body of an email or text message. The default setting is enabled.

   - **Allow users to configure call answering rules**: Select this check box to allow users who are associated with the UM mailbox policy to create call answering rules. If this option is disabled on the UM dial plan, this feature won't be available to UM-enabled users associated with the UM mailbox policy. The default setting is enabled.

   - **Allow message waiting indicator**: Select or clear this check box to enable or disable Message Waiting Indicator for users associated with the UM mailbox policy. Message Waiting Indicator is a feature found in most legacy voice mail systems. In its most common form, it lights a lamp on the voice mail user's phone to indicate the presence of a new voice message. Message Waiting Indicator can also send a text message to the UM-enabled user's mobile phone. The default setting is enabled.

   - **Allow Outlook Voice Access**: Select or clear this check box to enable or disable access to Outlook Voice Access for UM-enabled users who are associated with this UM mailbox policy. Outlook Voice Access is a feature used by UM-enabled users to access their mailbox over a phone. By default, this setting is enabled.

   - **Allow missed call notifications**: Select or clear this check box to enable or disable missed call notifications for users associated with the UM mailbox policy. A missed call notification is an email message sent to a user's mailbox when the user doesn't answer an incoming call. This is a different email message than the email message that contains the voice message left for a user.

     > [!NOTE]
     > When you're integrating Unified Messaging and Lync Server on-premises, missed call notifications aren't available to users who have a mailbox located on an Exchange 2007 or Exchange 2010 Mailbox server. A missed call notification is generated when a user disconnects before the call is sent to Unified Messaging.

     Typically, when a user misses an incoming call, the user receives two email messages: a message that contains the voice message and a missed call notification message. By default, missed call notifications are enabled when a UM mailbox policy is created.

   - **Allow Play on Phone for voice mail**: Select or clear this check box to enable or disable the Play on Phone feature for users associated with the UM mailbox policy. This option is enabled by default and allows users to play their voice messages over any phone, including an office or mobile phone.

   - **Allow inbound faxes**: Select or clear this check box to enable or disable inbound faxes for users associated with the UM mailbox policy. By default, when you enable users for UM, their mailbox is able to receive faxes. However, if this option is disabled on the UM dial plan, UM-enabled users associated with the UM mailbox policy won't be able to receive faxes. The default setting on the UM mailbox policy is disabled.

     After you have enabled the **Allow inbound faxes** setting, you will need to specify the URI for the partner fax server. If the UM mailbox policy is linked to a dial plan that can use TCP and TLS, you will need to enter URIs for both TCP and TLS.

   - **Help Microsoft improve voice mail preview**: These options allow Microsoft to improve the quality of Voice Mail Preview. You can enable the following settings:

   - **Allow analysis of voice messages left by callers**: Use this option to help improve the quality of Voice Mail Preview in future releases of Microsoft Exchange by forwarding copies of voice messages to Microsoft for analysis. You can't set this option if all voice messages are protected.

   - **Tell callers that voice messages may be analyzed**: Use this option to tell callers that the messages they leave may be analyzed by Microsoft to improve the quality of Voice Mail Preview, and allow them to opt out.

   - Use **Message Text** to configure message text settings for users who are associated with a UM mailbox policy. For example, you can specify the email message text sent to users after they reset their UM PIN. You can configure the following:

   - **When a user is enabled for Unified Messaging**: The text entered in this text box appears in the email message sent to users when they are enabled for UM. When a recipient's mailbox is enabled for UM and they are enabled for voice mail, an email message that welcomes the user to Unified Messaging is sent to the user. This text box is limited to 512 characters and can contain simple HTML formatting. By default, no text is defined in this text box.

     This welcome message contains welcome text and the PIN information that the user will use to access the UM or voice mail system. The text entered in this text box is included at the bottom of this welcome message. You can use this text box to include information such as the voice mail technical support telephone numbers or Outlook Voice Access numbers.

     If text isn't entered in this text box, the default text generated by the UM or voice mail system is included in the email message.

     The text that you provide in this text box can be plain. It can also contain simple HTML formatting tags if you want to emphasize text or add hyperlinks to other content.

     **Example 1**: If you have any questions or suggestions about voice mail service, please call the help desk at extension 4200.

     **Example 2**: If you have any questions or suggestions about \<b\>voice mail service\</b\>, please call the help desk at extension 4200 or visit our website at \<a href="http://emp.contoso.com/itinfo/vmail"\>\</a\>.

   - **When a user's Outlook Voice Access PIN is reset**: The text entered in this text box is included in the email message sent to UM-enabled users when their UM PIN is reset.

     A PIN is reset by the UM or voice mail system if the number of failed sign-in attempts exceeds 10 (by default) or if users reset their PIN using the UM features included with Microsoft Outlook, Outlook on the web (formerly known as Outlook Web App), or Outlook Voice Access from a telephone. You can use this text box to include information such as security notices or other security-related information in the email message.

     If text isn't entered in this text box, the default text generated by the UM system is included in the email message.

     This text box is limited to 512 characters. By default, no text is defined in this text box.

     The text that you provide in this text box can be plain. It can also contain simple HTML formatting tags if you want to emphasize text or add hyperlinks to other content.

   - **When a user receives a voice message**: The text entered in this text box is included in the email message sent to users when they receive a voice message from an incoming caller. For example, this text can include disclaimers that contain information about forwarding voice messages or system security policies that describe the correct way to handle voice messages in your organization.

     If text isn't entered in this text box, the default text generated by the system is included in the email message. This text box is limited to 512 characters. By default, no text is defined in this text box.

     The text that you provide in this text box can be plain. It can also contain simple HTML formatting tags if you want to emphasize text or add hyperlinks to other content.

   - **When a user receives a fax message**: The text entered in this text box is included in the email message sent to users when they receive an incoming fax message in their Inbox. You can use this text box to include disclaimers that contain information about forwarding fax messages or other system security policies about the correct way to handle fax messages in your organization.

     If text isn't entered in this text box, the default text generated by the system is included in the email message. This text box is limited to 512 characters. By default, no text is defined in this text box.

   - Use **PIN Policies** to configure PIN settings for users who are associated with a UM mailbox policy. UM PINs enable users to access their Inboxes by using a telephone. By configuring settings on this page, you can specify the minimum number of digits for a UM PIN or the number of failed sign-in attempts before users are locked out of their UM mailbox.

     Make sure that you plan carefully for the UM PIN policies that you implement in your environment. If you don't plan and implement the appropriate UM PIN policies, you may introduce security threats and mistakenly allow unauthorized access to your network. You can configure the following:

   - **Minimum PIN length (digits)**: Use this text box to specify the minimum number of digits that a UM user's PIN can contain. The default setting is six digits. The range is from 4 through 24 numeric digits. This setting can't be disabled.

     Increasing the number of digits required for a PIN increases the level of security for your UM system. Decreasing the number of digits required for a PIN reduces the level of security for your network. The fewer the digits that are required in a PIN, the easier it is for a potential attacker to guess a user's PIN.

     If this setting is set too high, users might have problems remembering their PINs. However, if the setting is too low, you risk unauthorized access to the UM system.

   - **PIN recycle count**: Use this setting to set the number of unique PINs that users must use before they can reuse an old PIN. For most organizations, this value should be set to the default of 5, the number of PINs that the system will remember. PIN history can't be disabled.

     You can set this value from 1 through 20. Setting this value too high can frustrate users because it can be difficult to memorize many PINs. Setting it too low may introduce a security threat to your network.

   - **Allow common PIN patterns**: Use this setting to set PIN complexity requirements for UM. These complexity requirements are enforced on PIN changes or when new PINs are created.

     If this option is disabled, sequential and repeated numbers and the suffix of the mailbox extension will be rejected. If this option is enabled, only the suffix of the mailbox extension will be rejected.

     As a security best practice, we recommend that you disable this setting. If this setting is disabled, user PINs can't contain the following:

     Sequential numbers, such as 123456 or 456789.

     Repeated numbers, such as 111111 or 8888888.

     Suffix of the mailbox extension.

   - **Enforce PIN lifetime (days)**: Use this text box to configure the number of days until the UM-enabled user's PIN expires. After the PIN expires, the user must create a new UM PIN. For most organizations, this value should be set to the default of 60 days.

     The value of this setting can be from 0 through 999. If it's set to 0, PINs never expire. Setting this value too low can frustrate users because they are required to create and memorize new PINS too frequently.

   - **Number of sign-in failures before PIN reset**: Use this text box to enter the number of sequential unsuccessful or failed sign-in attempts that can occur before the UM system automatically resets a user's PIN. For most organizations, this value should be set to the default of 5 attempts.

     The value of this setting can be from 0 through 999. If it's set to 0, this setting is disabled and the system won't automatically reset users' PINs. Setting this value too low can frustrate users; setting it too high gives malicious users more attempts to determine the PIN.

     This setting must be set to a number lower than the number configured in the **Number of sign-in failures before lockout** setting. This setting is designed to help prevent a brute force attack on user PINs.

   - **Number of sign-in failures before lockout**: Use this text box to enter the maximum number of sequential unsuccessful or failed sign-in attempts before users are locked out of their mailboxes.

     For example, if a user tries to sign in to the mailbox unsuccessfully five times, based on the **Number of sign-in failures before PIN reset** setting, the system will reset the user's PIN. If the user tries to use the new PIN five more times unsuccessfully, the system will again reset the PIN. If the user tries to use this new PIN five more times unsuccessfully, the user is then locked out of the mailbox. After a user is locked out, an administrator must manually reset or unlock the mailbox for the user.

     This value can be set from 1 through 999. Setting this value too low can frustrate users; setting it too high gives malicious users more attempts to determine the PIN. For most organizations, this value should be set to the default of 15 attempts.

     This number must be greater than the number set in the **Number of sign-in failures before PIN reset** setting. This setting is designed to help prevent a brute force attack on user PINs.

   - Use **Dialing authorization** to configure dialing rules for UM-enabled users who are associated with this UM mailbox policy.

     You can use these settings to control the extension numbers that can be reached or the telephone numbers that can be dialed by UM-enabled users who are associated with the UM mailbox policy. You can configure the following:

   - **Calls in the same UM dial plan**: Select this check box to allow UM-enabled users who call in to a subscriber access number configured on a dial plan and successfully sign in to their mailbox to place calls or transfer to UM-enabled users who have extension numbers within the same dial plan. By default, this setting is enabled.

     When you disable this setting, UM-enabled users who call in to a subscriber access number configured on a dial plan and successfully sign in to their mailbox can place calls or transfer calls to users who aren't UM-enabled or to other extension numbers not associated with a UM-enabled user. However, they can't transfer to UM-enabled users who are within the same dial plan. This is because the **Calls to any extension** setting is enabled by default.

   - **Calls to any extension**: When this setting is enabled, users who call in to a subscriber access number configured on a dial plan and successfully sign in to their mailbox can place calls to users who aren't UM-enabled, to other extension numbers not associated with a UM-enabled user, and to UM-enabled users within the same dial plan. This is because the **Calls in the same UM dial plan** setting is enabled by default.

     When this setting is disabled, users who call in to an Outlook Voice Access number configured on a dial plan and successfully sign in to their mailbox can't place calls to users who aren't UM-enabled or to other extension numbers not associated with a UM-enabled user. However, they can place calls or transfer calls to extension numbers associated with UM-enabled users. This is because the **Calls in the same UM dial plan** setting is enabled by default. The **Calls to any extension** setting is enabled by default.

     You can enable this setting in an environment where not all users have been UM-enabled. This setting is also useful when you want to allow users who call in to an Outlook Voice Access number configured on a dial plan to call extension numbers not associated with a UM-enabled user.

   - **Authorized in-country/region dialing rule groups**: Use this section to add or remove allowed in-country/region dialing rule groups. By default, there are no in-country/region dialing rule groups configured on UM mailbox policies.

     In-country/region dialing rule groups are used to allow or restrict the telephone numbers within a country or region that Outlook Voice Access users can dial. This helps prevent unnecessary or unauthorized telephone calls and charges.

     To add in-country/region dialing rule groups, you must first create the appropriate in-country/region dialing rule groups on the dial plan associated with the UM mailbox policy, and then add the appropriate dialing rule entries on the dialing rule group. After you create the required dialing rule groups on the dial plan, you must then add the dialing rule groups to the list of dialing restrictions under **Dialing authorization** on the UM mailbox policy.

     In-country/region dialing rule groups can be used to enable Unified Messaging to allow or restrict access to telephone numbers within a country or region. This is applied to Outlook Voice Access users who have called in to an Outlook Voice Access number.

   - **Authorized international dialing rule groups**: Use this section to add or remove allowed international dialing rule groups. By default, there are no international dialing rule groups configured on UM mailbox policies.

     To add international dialing rule groups, you must first create the appropriate international dialing rule groups on the dial plan associated with the UM mailbox policy, and then add the appropriate dialing rule entries on the dialing rule group. After you create the required dialing rule groups, you must add the dialing rule groups to the dialing restrictions on the UM mailbox policy.

     International dialing rule groups can be used to enable Unified Messaging to allow or restrict access to telephone numbers outside a country or region. This is applied to Outlook Voice Access users who have called in to a Outlook Voice Access number.

     International dialing rule groups are used to allow or restrict the telephone numbers outside a country or region that Outlook Voice Access users can dial. This helps prevent unnecessary or unauthorized telephone calls and charges.

   - Use **Protected Voice Mail** to configure the following settings:

   - **Protect voice messages from unauthenticated callers**: Select one of the following options from the drop-down list to determine whether an incoming call answered by Unified Messaging will protect voice messages. This setting applies to voice messages sent to UM-enabled users when they don't answer their phone. This setting also applies to voice messages sent directly to UM-enabled users when the caller uses a UM auto attendant. You can configure the following:

     **None**: Use this setting to not have protection applied to any voice messages sent to UM-enabled users.

     **Private**: Use this setting when you want to apply protection only to voice messages that have been marked as private by the caller.

     **All**: Use this setting when you want to apply protection to all voice messages, including those not marked as private.

   - **Protect voice messages from authenticated callers**: Select one of the following options from the drop-down list to determine whether an incoming call answered by Unified Messaging will protect voice messages. This setting applies to voice messages sent to UM-enabled users when they don't answer their phone. This setting also applies when callers sign in to their mailbox using Outlook Voice Access, and then create and send a voice message. You can configure the following:

     **None**: Use this setting to not have protection applied to any voice messages sent to UM-enabled users.

     **Private**: Use this setting when you want to apply protection only to voice messages that have been marked as private by the caller.

     **All**: Use this setting when you want to apply protection to all voice messages, including those not marked as private.

   - **Require Play on Phone for protected voice messages**: Select this check box if you want to force users who receive protected voice messages to use the Play on Phone feature. Or, if the client software doesn't support rights management, users must use Outlook Voice Access. The Play on Phone feature only applies to clients using a version of Outlook that supports rights management. For Outlook 2007 and earlier versions that don't support rights management, and for Outlook on the web clients, Outlook Voice Access is the only way that users can listen to protected voice mail.

     The default setting requires all users associated with the UM mailbox policy to use the Play on Phone feature to listen to voice messages that are protected. By doing this, it prevents other people from hearing the voice message from a media player over computer speakers or from a media player on a mobile phone. Even if this is enabled, a UM-enabled user can still use Outlook Voice Access to hear the protected voice mail.

     This is especially useful when UM-enabled users use public computers, laptops in public places, or their mobile phone's media player to listen to protected voice mail that can contain private information.

   - **Allow voice responses to email and calendar items**: Use this option to allow UM-enabled users to send voice responses to protected voice mail messages. The default is enabled. If you disable this, if a UM-enabled user receives a protected voice mail message, they will not be able to use Outlook Voice Access to reply to email and calendar items.

   - **Message to send to users who don't have Windows Rights Management support**: Protected voice mail can only be accessed by email clients that support Information Rights Management (IRM), or if a UM-enabled user uses Outlook Voice Access to access the protected voice mail message.

     If a protected voice mail message is sent to an email client that doesn't support IRM, the text that you include in this box will be sent to the user in an email message. This information should include instructions about what to do to be able to receive the protected voice mail message.

## Use Exchange Online PowerShell to manage a UM mailbox policy

This example sets the PIN settings for users who are associated with a UM mailbox policy named `MyUMMailboxPolicy`.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -LogonFailuresBeforePINReset 8 -MaxLogonAttempts 12 -MinPINLength 8 -PINHistoryCount 10 -PINLifetime 60 -ResetPINText "The PIN that is used to allow you access to your mailbox using Outlook Voice Access has been reset."
```

This example selects the in-country or region groups and international groups from those configured on the UM dial plan associated with the UM mailbox policy. UM-enabled users associated with this UM mailbox policy will be able to place outbound calls according to the rules defined on these groups.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowDialPlanSubscribers $true -AllowedInCountryOrRegionGroups InCountry/RegionGroup1,InCountry/RegionGroup2 -AllowedInternationalGroups InternationalGroup1,InternationalGroup2 -AllowExtensions $true
```

This example configures the text of voice messages sent to UM-enabled users and the text included in an email message sent to a user who has been UM-enabled.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -UMEnabledText "You have been enabled for Unified Messaging." -VoiceMailText "You have received a voice message from Microsoft Exchange Unified Messaging."
```

## Use Exchange Online PowerShell to view UM mailbox policy properties

This example returns a formatted list of all UM mailbox policies in the Active Directory forest.

```PowerShell
Get-UMMailboxPolicy | Format-List
```

This example returns the properties and values for a UM mailbox policy named `MyUMMailboxPolicy`.

```PowerShell
Get-UMMailboxPolicy -Identity MyUMMailboxPolicy
```
