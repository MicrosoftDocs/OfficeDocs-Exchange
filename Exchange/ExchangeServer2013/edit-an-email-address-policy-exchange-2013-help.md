---
title: 'Edit an email address policy: Exchange 2013 Help'
TOCTitle: Edit an email address policy
ms:assetid: cc8b36a0-95f4-43e9-bc64-87646d2e14e4
ms:mtpsurl: https://technet.microsoft.com/library/Bb124580(v=EXCHG.150)
ms:contentKeyID: 49289411
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.EditEmailAddressPolicyWizardForm.EmailAddressPolicyIntroductionPage
---

# Edit an email address policy

_**Applies to:** Exchange Server 2013_

Email address policies generate the primary and secondary email addresses for your recipients (which include users, contacts, and groups) so they can receive and send email.

For additional management tasks related to email address policies, see [Email address policy procedures](email-address-policy-procedures-exchange-2013-help.md).

## What do you need to know before begin?

- Estimated time to complete: 5 minutes.

- You can't use the Exchange admin center (EAC) to edit an email address policy if the policy was created by using the Shell.

- If the email address policy was created using a recipient filter, you must use the Shell to edit the email address policy. For more information, see [Create an email address policy by using recipient filters](create-an-email-address-policy-by-using-recipient-filters-exchange-2013-help.md).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Email address policies" entry in the [Email addresses and address books](email-addresses-and-address-books-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!WARNING]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to change the recipients that the policy applies to

1. Navigate to **Mail flow** \> **Email address policies**.

2. In the list view, select the email address policy you want to change, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In **Email Address Policy**, click **Apply to** and modify the settings.

## Use the EAC to change the email address policy's priority

A user can have multiple proxy email addresses for the same email account (for example, ayla@exchange.mail.contoso.com or ayla@contoso.com). These email addresses can then be applied by priority. For example, consider this scenario: you have two email address policies, and you assign them priorities of 1 and 2. If you create another policy, it will automatically be assigned a priority of 3. However, let's say you have two policies, and you specify that one of them is priority 1, but the other policy was assigned a default priority of 2 when it was created. In this case, the next policy you create will, by default, become the priority 2 policy. The previous priority 2 policy will be assigned a priority of 3.

1. Navigate to **Mail flow** \> **Email address policies**.

2. Select the email address policy for which you want to change the priority, and then click **Increase priority** ![Up Arrow Icon](images/JJ150576.1732c727-328b-4a1a-b84d-6d7252c7dcab(EXCHG.150).gif "Up Arrow Icon") or **Decrease priority** ![Down Arrow Icon](images/JJ150576.ef5ca57d-a033-457b-bd92-6361877c33d0(EXCHG.150).gif "Down Arrow Icon").

## Use the Shell to edit an email address policy

This example edits the email address policy South East Offices that currently includes recipients in Georgia, Alabama, and Louisiana to also include recipients in Texas.

```powershell
Set-EmailAddressPolicy -Identity "South East Offices" -ConditionalStateorProvince "Georgia","Alabama","Louisiana","Texas"
```

> [!NOTE]
> Although the email address policy is already applied to recipients in Georgia, Alabama, and Louisiana, you must include them in the parameter because the parameter overwrites values; it doesn't append values to existing ones.

For detailed syntax and parameter information, see [Set-EmailAddressPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-EmailAddressPolicy).
