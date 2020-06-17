---
description: 'Summary: Learn about changing a mailbox from one type to another in Exchange Server 2016 or Exchange Server 2019.'
localization_priority: Normal
ms.author: v-mapenn
ms.topic: article
author: mattpennathe3rd
ms.reviewer:
manager: serdars
ms.prod: exchange-server-it-pro
ms.assetid: dfed045e-a740-4a90-aff9-c58d53592f79
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
title: Convert a mailbox in Exchange Server

---

# Convert a mailbox in Exchange Server

In Exchange Server 2013 or later, converting a mailbox from one type of mailbox to another is mostly unchanged from the experience in Exchange 2010. You still need to use the **Set-Mailbox** cmdlet in the Exchange Management Shell to do the conversion.

You can convert the following mailboxes to a different type:

- User mailbox to room or equipment mailbox

- User mailbox to shared mailbox

- Shared mailbox to user mailbox

- Shared mailbox to room or equipment mailbox

- Room or equipment mailbox to user mailbox

- Room or equipment mailbox to shared mailbox

 **Note:** If your organization uses a hybrid Exchange environment, you need to manage your mailboxes by using the on-premises Exchange management tools. To convert a mailbox in a hybrid environment, you might need to move the mailbox back to on-premises Exchange, convert the mailbox type, and then move it back to Office 365.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- Room, equipment, and shared mailboxes have associated user accounts in Active Directory, but the accounts are disabled. When you convert one of these mailbox types to a regular (user) mailbox, you need to specify a password that satisfies the length and complexity requirements for your organization.

    Overwriting an existing password requires the Reset Password role, which isn't assigned to any role groups by default. To assign the role to a role group that you belong to, see [Add a role to a role group](../../permissions/role-groups.md#add-a-role-to-a-role-group). Note that changes in permission require you to log off and log on for the changes to take effect.

- When you convert a regular (user) mailbox to a room, equipment, or shared mailbox, the associated account is disabled.

    For room mailboxes, you can enable the associated user account, which also requires you to specify a password (which requires the Reset Password role). You need to enable the room mailbox user account for features like the Skype for Business Room System.

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to convert a mailbox

To convert a mailbox to a different type, use this syntax:

```PowerShell
Set-Mailbox -Identity <MailboxIdentity> -Type <Regular | Room | Equipment | Shared> [-Password (ConvertTo-SecureString -String '<Password>' -AsPlainText -Force)] [-EnableRoomMailboxAccount <$true | $false>] [-RoomMailboxPassword (ConvertTo-SecureString -String '<Password>' -AsPlainText -Force)] [-ResetPasswordOnNextLogon <$true | $false>]
```

This example converts the shared mailbox named Marketing Dept 01 to a user mailbox with the new password P@ssw0rd25, and the requirement to change the password the next time the user logs in to the mailbox.

```PowerShell
Set-Mailbox -Identity "Marketing Dept 01" -Type Regular -Password (ConvertTo-SecureString -String 'P@ssw0rd25' -AsPlainText -Force) -ResetPasswordOnNextLogon $true
```

This example converts the user mailbox named Conference Room 01 to a room mailbox.

```PowerShell
Set-Mailbox -Identity "Conference Room 01" -Type Room
```

This is the same example, but the user account for the room mailbox is enabled, and the password is P@ssw0rd25

```PowerShell
Set-Mailbox -Identity "Conference Room 01" -Type Room -EnableRoomMailboxAccount $true -RoomMailboxPassword (ConvertTo-SecureString -String 'P@ssw0rd25' -AsPlainText -Force)
```

 **Note**: Even when you convert a user mailbox with a known password to a room mailbox, you still need to use the _RoomMailboxPassword_ parameter to specify a password.

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

## How do you know this worked?

To verify that you've successfully converted a mailbox, replace _\<MailboxIdentity\>_ with the name, alias, or email address of the mailbox, and run this command in the Exchange Management Shell to verify the property values:

```PowerShell
Get-Mailbox -Identity <MailboxIdentity> | Format-List Name,RecipientTypeDetails,UserPrincipalName,AccountDisabled
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox).
