---
title: 'Policy Tips: Exchange 2013 Help'
TOCTitle: Policy Tips
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer: 
ms.assetid: 4266b83c-dd8a-4b3d-99ff-402e68fc810c
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Policy Tips in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can help to prevent your organization's Microsoft Outlook, Outlook Web App (OWA), and OWA for Devices email users from inappropriately sending sensitive information by creating data loss prevention (DLP) policies that include Policy Tip notification messages. Similar to MailTips that were introduced in Microsoft Exchange Server 2010, Policy Tip notification messages are displayed to users in Outlook while they are composing an email message. Policy Tip notification messages only show up if something about the sender's email message seems to violate a DLP policy that you have in place and that policy includes a rule to notify the sender when the conditions that you establish are met. Watch this video to learn more.

[Policy Tips](https://www.microsoft.com/videoplayer/embed/dd629bb7-063d-49f3-b7e1-8f2e0aa4de13?autoplay=false)

In order to show Policy Tips to your email senders, your rules must include the **Notify the sender with a Policy Tip** action. You can add this in the rules editor from the Exchange admin center. For more information, see [Manage policy tips](manage-policy-tips-exchange-2013-help.md).

The transport rule agent that enforces DLP policies does not differentiate between email message attachments, body text, or subject lines while evaluating messages and the conditions within your policies. For example, if a user creates an email message that includes a credit card number in the body of the message and then attempts to address the message to a recipient outside your organization, then a Policy Tip notification message can be shown to that user in Outlook or Outlook Web App reminding them of your enterprise's expectations for such information. However, this type of notification will only show up if you have configured a DLP policy that restricts the example actions described; in this case adding an external email alias to the header of a message with credit card data. There is a great variety of conditions, actions, and exceptions you can choose from while creating DLP policies. This variety allows you to tailor your data loss prevention efforts in a way that meets your specific organization's needs.

Any time you use either the notify sender action or an override action within a rule, we recommend that you also include the condition that the message was sent from within your organization. You can do this by using the policy rules editor to add the following condition: **The sender is located...** \> **inside the organization**. Learn more about changing existing DLP policies at [Manage DLP policies](manage-dlp-policies-exchange-2013-help.md). This is a best practice recommendation because the notify sender action is applied as part of your company's message creation experience. The senders referred to by the action are the authors of messages within your company. The user interaction presented by Policy Tips cannot be acted upon by your users for incoming messages and will be ignored when the sender is located outside your organization. You can apply DLP policies to scan incoming messages and take a variety of actions, but when you do this, don't add the notify sender action.

If email senders in your organization who are in the act of composing a message are made aware of your organizational expectations and standards in real time through Policy Tip notifications, then they are less likely to violate standards that your organization wants to enforce.

> [!NOTE]
> DLP is a premium feature that requires an Exchange Enterprise Client Access License (CAL). For more information about CALs and server licensing, see [Exchange licensing FAQs](https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-server-licensing-licensing-overview). <br/> If your organization is using Exchange 2013 SP1, Policy Tips are available to people sending mail from Outlook 2013, Outlook Web App, or OWA for Devices. However, if your organization is currently using Exchange 2013, Policy Tips are only available to people sending email from Outlook 2013.

## Default text for Policy Tips and rule options

You have a range of possible options when you add sender notification rules to DLP policies. When you add a rule to notify the sender by using the **Notify the sender with a Policy Tip** action within a DLP policy, you can choose how restrictive to be. The notification options in the following table are available. For general information about editing policies, see [Manage DLP policies](manage-dlp-policies-exchange-2013-help.md). For specific information about creating Policy Tips, see [Manage policy tips](manage-policy-tips-exchange-2013-help.md).

|**Notification rule**|**Meaning**|**Default Policy Tip notification message that Outlook users will see**|
|:-----|:-----|:-----|
|**Notify only**|Similar to MailTips, this causes an informative Policy Tip notification message about a policy violation. A sender can prevent this type of tip from showing up by using a Policy Tip options dialog box that can be accessed in Outlook.|This message may contain sensitive content. All recipients must be authorized to receive this content.|
|**Reject message**|The message will not be delivered until the condition is no longer present. The sender is provided with an option to indicate that their email message does not contain sensitive content. This is also known as a false-positive override. If the sender indicates this, then Outlook will allow the message to leave the outbox so that the user's report may be audited, but Exchange will block the message from being sent.|This message may contain sensitive content. Your organization won't allow this message to be sent until that content is removed.|
|**Reject unless false positive override**|The result with this notification rule is similar to the **Reject message** notification rule. However, if you select this then Exchange will allow the message to be sent to the intended recipient, instead of blocking the message.|**Before the sender selects an option to override:** This message may contain sensitive content. Your organization won't allow this message to be sent until that content is removed.  <br/> **After the sender selects an option override:** Your feedback will be submitted to your administrator when the message is sent.|
|**Reject unless silent override**|The message will not be delivered until the condition is no longer present or the sender indicates an override. The sender is provided with an option to indicate that they wish to override the policy.|**Before the sender selects an option to override:** This message may contain sensitive content. Your organization won't allow this message to be sent until that content is removed.  <br/> **After the sender selects an option override:** You have overridden your organization's policy for sensitive content in this message. Your action will be audited by your organization.|
|**Reject unless explicit override**|The result with this notification rule is similar to the **Reject unless silent override** notification rule, except that in this case when the sender attempts to override the policy, they are required to provide a justification for overriding the policy.|**Before the sender selects an option to override:** This message may contain sensitive content. Your organization won't allow this message to be sent until that content is removed.  <br/> **After the sender selects an option override:** You have overridden your organization's policy for sensitive content in this message. Your action will be audited by your organization.|

## Customize your Policy Tip notification messages

To customize the text of a Policy Tip notification that email senders see in their email program, select **Manage Policy Tips** on the Data Loss Prevention page. In order for any of your custom text to appear, a DLP policy rule must include the **Notify the sender with a Policy Tip** action. Add the action to a rule by using the DLP rules editor.

For procedures that explain how to create your own Policy Tips, see [Manage policy tips](manage-policy-tips-exchange-2013-help.md). The custom text that you create can replace the default text shown in the previous table.

|**Policy Tip Notification Actions and Settings**|**Meaning**|
|:-----|:-----|
|**Notify the sender**|Your text only appears when a **Notify the sender, but allow them to send** action is initiated.|
|**Allow the sender to override**|Your text only appears when the following actions are initiated: **Block the message unless it's a false positive**, **Block the message, but allow the sender to override and send**.|
|**Block the message**|Your text only appears when a **Block the message** action is initiated.|
|**Link to compliance URL**|The compliance URL is a link to a web page where you can explain your compliance and override policies. This link is displayed in the Policy Tip when a user clicks the **More details** link.|

## For more information

[Data loss prevention](data-loss-prevention-exchange-2013-help.md)

[Manage DLP policies](manage-dlp-policies-exchange-2013-help.md)

[Manage policy tips](manage-policy-tips-exchange-2013-help.md)
