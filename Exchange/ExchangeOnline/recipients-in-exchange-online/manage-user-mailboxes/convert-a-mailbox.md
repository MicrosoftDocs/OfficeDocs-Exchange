---
localization_priority: Normal
description: Converting a mailbox to a different type of mailbox is very similar to the experience in earlier versions of Exchange. You must still use the Set-Mailbox cmdlet in Exchange Online PowerShell to do the conversion.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: dfed045e-a740-4a90-aff9-c58d53592f79
ms.reviewer: 
f1.keywords:
- NOCSH
title: Convert a mailbox
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Convert a mailbox using new Exchange admin center

1. In the new EAC, navigate to **Recipients > Mailboxes**.
   The **Mailboxes** page is displayed.
   
2. Select a mailbox that you want to convert, and click the display name.
   The properties page is displayed.
   
3. Under **More actions**, do the following:

   - To convert from regular to shared mailbox, click **Convert to shared mailbox** and then click **Confirm**.
   
   - To convert from shared to regular, click **Convert to regular mailbox** and then click **Confirm**.
   
 The notification message **Mailbox converted successfully** is displayed.
   
## Convert a mailbox using Powershell

Converting a mailbox to a different type of mailbox is very similar to the experience in earlier versions of Exchange. You must still use the Set-Mailbox cmdlet in Exchange Online PowerShell to do the conversion.

You can convert the following mailboxes from one type to another:

- User mailbox to resource (room or equipment) mailbox

- Shared mailbox to user mailbox

- Shared mailbox to resource mailbox

- Resource mailbox to user mailbox

- Resource mailbox to shared mailbox

Note that if your organization uses a hybrid Exchange environment, you need to manage your mailboxes by using the on-premises Exchange management tools. To convert a mailbox in a hybrid environment, you might need to move the mailbox back to on-premises Exchange, convert the mailbox type, and then move it back to Microsoft 365 or Office 365.

> [!IMPORTANT]
> If you are converting a user mailbox to a shared mailbox, you should either remove any mobile devices from the mailbox before the conversion, or you should block mobile access to the mailbox after the conversion. This is because once the mailbox is converted to a shared mailbox, mobile functionality will not work properly. Additionally, if you are trying to prevent access to the converted mailbox, you might have to reset the password. For more information on blocking access, see [Remove a former employee](https://docs.microsoft.com/microsoft-365/admin/add-users/remove-former-employee). <br/><br/> Delegated Access Permission (DAP) partners with Administer On Behalf Of (AOBO) permissions can't use the procedures in this topic to convert customer mailboxes. Only members of the Organization Management role group in Exchange Online (global admins) can convert mailboxes.

## Use Exchange Online PowerShell to convert a mailbox

Estimated time to complete: 5 minutes.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

This example converts the shared mailbox, MarketingDept1 to a user mailbox.

```PowerShell
Set-Mailbox MarketingDept1 -Type Regular
```

You can use the following values for the _Type_ parameter:

- Regular

- Room

- Equipment

- Shared

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

## How do you know this worked?

To verify that you have successfully converted the mailbox, run the following command in Exchange Online PowerShell:

```PowerShell
Get-Mailbox -Identity MarketingDept1 | Format-List RecipientTypeDetails
```

The value for _RecipientTypeDetails_ should be `UserMailbox`.

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://docs.microsoft.com/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).
