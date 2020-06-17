---
title: 'Create an Email Address Policy: Exchange 2013 Help'
TOCTitle: Create an Email Address Policy
ms:assetid: eb2bf42e-2058-4e17-85d5-97546433b40a
ms:mtpsurl: https://technet.microsoft.com/library/Bb125137(v=EXCHG.150)
ms:contentKeyID: 49289449
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.NewEmailAddressPolicyWizardForm.EmailAddressPolicyIntroductionPage
---

# Create an Email Address Policy

_**Applies to:** Exchange Server 2013_

For a recipient to receive or send email messages, the recipient must have an email address. Email address policies generate the primary and secondary email addresses for your recipients (which include users, contacts, and groups) so they can receive and send email.

When creating an email address policy, you can use the following email address types:

- **Precanned SMTP email address**. *Precanned* SMTP email addresses are commonly used email address types that are provided for you.

- **Custom SMTP email address**. If you don't want to use one of the precanned SMTP email addresses, you can specify a custom SMTP email address.

    When creating a custom SMTP email address, you can use the variables in the following table to specify alternate values for the local part of the email address.

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Variable</th>
    <th>Value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>%g</p></td>
    <td><p>Given name (first name)</p></td>
    </tr>
    <tr class="even">
    <td><p>%i</p></td>
    <td><p>Middle initial</p></td>
    </tr>
    <tr class="odd">
    <td><p>%s</p></td>
    <td><p>Surname (last name)</p></td>
    </tr>
    <tr class="even">
    <td><p>%d</p></td>
    <td><p>Display name</p></td>
    </tr>
    <tr class="odd">
    <td><p>%m</p></td>
    <td><p>Exchange alias</p></td>
    </tr>
    <tr class="even">
    <td><p>%<em>x</em>s</p></td>
    <td><p>Uses the first <em>x</em> letters of the surname. For example, if <em>x</em> = 2, the first two letters of the surname are used.</p></td>
    </tr>
    <tr class="odd">
    <td><p>%<em>x</em>g</p></td>
    <td><p>Uses the first <em>x</em> letters of the given name. For example, if <em>x</em> = 2, the first two letters of the given name are used.</p></td>
    </tr>
    </tbody>
    </table>

- **Non-SMTP email address**. The following types of non-SMTP email addresses are supported:

  - EX (Legacy DN Proxy Address Prefix DisplayName)

  - X.500

  - X.400

  - MSMail

  - CcMail

  - Lotus Notes

  - Novell GroupWise

  - Exchange Unified Messaging proxy address (EUM proxy address)

    > [!IMPORTANT]
    > In Exchange, all non-SMTP email addresses are considered custom addresses. Exchange doesn't provide unique dialog boxes or property pages for X.400, GroupWise, or Lotus Notes email address types. If you add a non-SMTP custom email address, you must have the appropriate dynamic-link library (DLL) files. If you don't provide the appropriate DLL files, you won't be able to create a customized email address policy. The following error will be logged in Event Viewer: "The email address description object in the Microsoft Exchange directory for the 'SADF' address type on 'i386' machines are missing."

For detailed instructions about how to create an email address policy, see the following topics:

[Create an Email Address Policy](create-an-email-address-policy-exchange-2013-help.md)

[Create an email address policy by using recipient filters](create-an-email-address-policy-by-using-recipient-filters-exchange-2013-help.md)

## What do you need to know before begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Email address policies" entry in the [Email addresses and address books](email-addresses-and-address-books-exchange-2013-help.md) topic.

- Before an SMTP address domain can be used in an email address policy, you must configure an accepted domain. To learn more, see [Accepted domains](accepted-domains-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!WARNING]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to create an email address policy

1. Navigate to **Mail flow** \> **Email address policies**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In **Email Address Policy**, complete the following fields:

   - **Policy name**

   - **Email address format**

   - **Specify the types of recipients this email address will apply to**

3. Click **Add a rule** to further restrict the recipients that this policy will apply to. This creates a Boolean **And** statement.

    > [!CAUTION]
    > If you apply too many rules, it's possible to restrict the email address policy to the point that it doesn't contain any users.

4. Click **Preview recipients the policy applies to** to view the recipients that policy will apply to.

5. Click **Save** to save your changes and create the policy.

6. You'll get a warning that the email address policy won't be applied until you update it. After it's created, select it, and then, in the details pane, click **Apply**.

## Use the Shell to create an email address policy

This example creates an email address policy that includes mailbox users in the Southeast offices who will have email addresses that include their last name combined with the first two letters of their first name.

```powershell
New-EmailAddressPolicy -Name "southeast offices" -IncludedRecipients MailboxUsers -ConditionalStateorProvince "Georgia","Alabama","Louisiana" -EnabledEmailAddressTemplates "SMTP:%s%2g@southeast.contoso.com"
```

For detailed syntax and parameter information, see [New-EmailAddressPolicy](https://docs.microsoft.com/powershell/module/exchange/New-EmailAddressPolicy).
