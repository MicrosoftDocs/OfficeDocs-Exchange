---
title: 'Configure custom MailTips for recipients: Exchange 2013 Help'
TOCTitle: Configure custom MailTips for recipients
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: df8ee7ae-2486-4890-b057-cda87b4cb1ec
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure custom MailTips for recipients in Exchange 2013

_**Applies to:** Exchange Server 2013_

MailTips are informative messages displayed to users in the InfoBar in Outlook Web App and Microsoft Outlook 2010 or later when a user does any of the following while composing an e-mail message:

- Add a recipient

- Add an attachment

- Reply or Reply all

- Open a message from the Drafts folder that's already addressed to recipients

In addition to the built-in MailTips that are available, you can create custom MailTips for all types of recipients. For more information about the built-in MailTips, see [MailTips](mailtips-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "MailTips" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- You can configure the primary MailTip in the Exchange admin center (EAC) or in the Shell. However, you can only configure additional MailTip translations in the Shell.

- When you add a MailTip to a recipient, two things happen:

  - HTML tags are automatically added to the text. For example, if you enter the text: `This mailbox is not monitored`, the MailTip automatically becomes: `<html><body>This mailbox is not monitored</body></html>`. Additional HTML tags in the MailTip aren't supported.

  - The text is automatically added to the _MailTipTranslations_ property of the recipient as the default value. If you modify the MailTip text, the default value is automatically updated in the _MailTipTranslations_ property.

- The length of a MailTip can't exceed 175 displayed characters.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Configure MailTips for recipients

### Use the EAC to configure MailTips for recipients

1. In the EAC, navigate to **Recipients**.

2. Select any of the following recipient tabs based on the recipient type:

   - **Mailboxes**

   - **Groups**

   - **Resources**

   - **Contacts**

   - **Shared**

3. On the recipient tab, select the recipient you want to modify, and click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

4. In the recipient properties page that appears, click **MailTips**.

5. Enter the text for the MailTip. When you are finished, click **Save**.

### Use the Shell to configure MailTips for recipients

To configure a MailTip for a recipient, use the following syntax.

```powershell
Set-<RecipientType> <RecipientIdentity> -MailTip "<MailTip text>"
```

 _\<RecipientType\>_ can be any type of recipient. For example, `Mailbox`, `MailUser`, `MailContact`, `DistributionGroup`, or `DynamicDistributionGroup`.

For example, suppose you have a mailbox named "Help Desk" for users to submit support requests, and the promised response time is two hours. To configure a custom MailTip that explains this, run the following command:

```powershell
Set-Mailbox "Help Desk" -MailTip "A Help Desk representative will contact you within 2 hours."
```

## Use the Shell to configure additional MailTips in different languages

To configure additional MailTip translations without affecting the existing MailTip text or other existing MailTip translations, use the following syntax:

```powershell
Set-<RecipientType> -MailTipTranslations @{Add="<culture1>:<localized text 1>","<culture2>:<localized text 2>"...; Remove="<culture1>:<localized text 1>","<culture2>:<localized text 2>"...}
```

 _\<culture\>_ is a valid ISO 639 two-letter culture code associated with the language.

For example, suppose the mailbox named Notifications currently has the MailTip: "This mailbox is not monitored." To add the Spanish translation, run the following command:

```powershell
Set-Mailbox -MailTipTranslations @{Add="ES:Esta caja no se supervisa."}
```

## How do you know this worked?

To verify that you have successfully configured a MailTip for a recipient, do the following:

1. In Outlook Web App or Outlook 2010 or later, compose an email message addressed to the recipient, but don't send it.

2. Verify the MailTip appears in the InfoBar.

3. If you configured additional MailTip translations, compose the message in Outlook Web App where the language setting matches the language of the MailTip translation to verify the results.
