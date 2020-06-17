---
title: 'Remove an email address policy: Exchange 2013 Help'
TOCTitle: Remove an email address policy
ms:assetid: f1d05223-7d41-406d-8fae-f4227be1c1c2
ms:mtpsurl: https://technet.microsoft.com/library/Bb125181(v=EXCHG.150)
ms:contentKeyID: 49289454
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove an email address policy

_**Applies to:** Exchange Server 2013_

By default, Exchange contains an email address policy that specifies the recipient's alias as the local part of the email address and uses the default accepted domain. The local part of an email address is the name that appears before the "at" sign (@).This email address policy applies to all users in the organization. You can't remove this email address policy.

For additional management tasks related to e-mail address policies, see [Email address policy procedures](email-address-policy-procedures-exchange-2013-help.md).

## What do you need to know before begin?

  - Estimated time to complete: 5 minutes.

  - If you remove an email address policy that's used by recipients as the primary policy and no other policies have been configured for recipients, the default policy will be used.

  - You can't delete the default policy. If you want to delete the default policy, you must first assign a different policy as the default.

  - If the email address policy you're deleting contains more than 3,000 recipients, you should use the Shell to perform this procedure.

  - You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Email address policies" entry in the [Email addresses and address books](email-addresses-and-address-books-exchange-2013-help.md) topic.

  - For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!WARNING]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to remove an email address policy

1. Navigate to **Mail flow** \> **Email address policies**.

2. In the list view, select the email address policy that you want to delete and then click **Delete** ![Delete icon](images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif "Delete icon").

3. In the warning, click **Yes** to remove the policy.

## Use the Shell to remove an email address policy

This example removes the e-mail address policy South East Offices.

```powershell
Remove-EmailAddressPolicy -Identity "South East Offices"
```

Type **Y** to confirm that you want to remove the policy, and then press ENTER.

For detailed syntax and parameter information, see [Remove-EmailAddressPolicy](https://docs.microsoft.com/powershell/module/exchange/Remove-EmailAddressPolicy).
