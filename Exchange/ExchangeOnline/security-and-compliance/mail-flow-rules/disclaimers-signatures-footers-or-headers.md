---
localization_priority: Normal
description: 'Summary: Admins can learn how to apply text to the top or bottom of outbound messages in Exchange Online'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 29ac61c2-77f1-4071-b14e-8cc64e3e76ba
ms.reviewer: 
f1.keywords:
- NOCSH
title: Organization-wide message disclaimers, signatures, footers, or headers in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Organization-wide message disclaimers, signatures, footers, or headers in Exchange Online

You can add an HTML or plain text legal disclaimer, disclosure statement, signature, or other information to the top or bottom of email messages that enter or leave your organization. To do this, you create a mail flow rule (also known as a transport rule) that adds the required information to messages.

**Notes**:

- Users can apply signatures to their own outgoing messages in Outlook or Outlook on the web (formerly known as Outlook Web App). For more information, see [Create and add an email signature in Outlook on the web](https://support.office.com/article/0F230564-11B9-4239-83DE-F10CBE4DFDFC).

- If you want the information to be added only to outgoing messages, you need to add a corresponding condition (for example, recipients located outside the organization). By default, mail flow rules are applied to incoming and outgoing messages.

- To avoid multiple disclaimers being added in an email conversation, add an exception that looks for unique text in your disclaimer. This ensures that the disclaimer is only added to the original message.

- Test the disclaimer. When you create the mail flow rule, you have the option to start using it immediately ( **Enforce**), or to test it first and view the results in the messaging log. We recommend testing all mail flow rules prior to setting them to **Enforce**.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 7 minutes.

- For information about how to access the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?linkid=396554).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail flow" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to add a disclaimer or other email header or footer

1. Open the EAC and go to **Mail flow** \> **Rules**.

2. Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif), and then click **Apply disclaimers**.

   ![In the Exchange admin center, click Mal flow \> Rules \> Add to create a rule](../../media/ee818b31-f5a5-40cc-9fe5-6c04f4120716.png)

3. In the **New rule** window that appears, enter a unique name the rule.

4. In the **Apply this rule if** box, select the conditions for displaying the disclaimer. For example, select **The recipient is located** condition, and then select **Outside the organization**. If you want this rule to apply to every message that enters or leaves your organization, select **[Apply to all messages]**.

5. Next to the **Do the following** box, select **Enter text** to enter the text of your disclaimer.

6. Click **Select one**, and select one of the [Fallback options if the disclaimer can't be added](https://docs.microsoft.com/Exchange/policy-and-compliance/mail-flow-rules/signatures#fallback-options-for-disclaimer-rules).

7. Specify the audit severity level to assign the severity level that appears in the message log.

8. Select the mode for the rule. Select **Enforce** to turn on the disclaimer immediately, or select **Test without Policy Tips** to put a message in the message tracking log instead of adding the disclaimer.

9. If you have additional conditions or exceptions that you want to add, select **More options** at the bottom of the page, which will show additional settings. For example, to add the exception that prevents multiple disclaimers being added in an email conversation, select **Add exception** and then select **The subject or body** \> **Subject or body matches these text patterns**, and then specify the words or phrases in your disclaimer. Or, to put your disclaimer at the top of the email message instead of the bottom, in **Do the following**, select **Apply a disclaimer to the message** \> **prepend a disclaimer**.

10. When you're finished, click **Save**.

## Use Exchange Online PowerShell to add a disclaimer or other email header or footer

Use the [New-TransportRule](https://docs.microsoft.com/powershell/module/exchange/new-transportrule) cmdlet to create the disclaimer rule. For detailed parameter information, see [Mail flow rule conditions and exceptions (predicates) in Exchange Online](conditions-and-exceptions.md).

This example creates a new mail flow rule that adds a disclaimer with an image to the end of all email messages that are sent outside the organization.

```PowerShell
New-TransportRule -Name "External Disclaimer" -SentToScope NotInOrganization -ApplyHtmlDisclaimerText "<h3>Disclaimer Title</h3><p>This is the disclaimer text.</p><img alt='Contoso logo' src='http://www.contoso.com/images/logo.gif'>"
```

This example creates a new mail flow rule that adds an advertisement for one month to the beginning of all outgoing messages.

```PowerShell
New-TransportRule -Name "March Special" -Enabled $true -SentToScope NotInOrganization -ApplyHtmlDisclaimerLocation Prepend -ActivationDate '03/1/2017' -ExpiryDate '03/31/2017'-ApplyHtmlDisclaimerText "<table align=center width=200 border=1 bordercolor=blue bgcolor=green cellpadding=10 cellspacing=0><tr><td nowrap><a href=http://www.contoso.com/marchspecials.htm>Click to see March specials</a></td></tr></table>"
```

## How do you know this worked?

To verify that you've successfully created a disclaimer, and that the disclaimer works as expected, do the following steps:

- Send yourself both a plain text email and an HTML email that match the conditions and exceptions you defined, and verify that the text appears as you intended.

- If you added an exception to avoid adding the disclaimer to successive messages in a conversation, forward your test messages to yourself to make sure that they don't get an extra copy of the disclaimer.

- Send yourself some messages that should not get the disclaimer and verify that the disclaimer is not included.

## For more information

After you configure a disclaimer or email header or footer, see [Manage mail flow rules](manage-mail-flow-rules.md) for information about how to view, modify, enable, disable, or remove a rule.
