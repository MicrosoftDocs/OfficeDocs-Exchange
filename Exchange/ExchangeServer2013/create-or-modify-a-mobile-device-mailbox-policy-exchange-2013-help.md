---
title: 'Create or modify a mobile device mailbox policy: Exchange 2013 Help'
TOCTitle: Create or modify a mobile device mailbox policy
ms:assetid: b4a37a81-25e3-40ff-a18a-a62ae4493635
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Bb124315(v=EXCHG.150)
ms:contentKeyID: 49345057
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Create or modify a mobile device mailbox policy

_**Applies to:** Exchange Server 2013_

A mobile device mailbox policy allows you to apply a common set of security and mobile device settings to a group of users. You can create multiple mobile device mailbox policies. Each recipient in your organization must have a mobile device mailbox policy assigned to them. When you install Microsoft Exchange Server 2013, a default mobile device mailbox policy is created and new users are automatically assigned this policy. To assign specific users to a mobile device mailbox policy, see [Add or remove users from a mobile mailbox policy](add-or-remove-users-from-a-mobile-mailbox-policy-exchange-2013-help.md).

For additional information related to mobile device mailbox policies, see [Mobile device mailbox policies](mobile-device-mailbox-policies-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mobile Device mailbox policy" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a new mobile device mailbox policy

You can use the EAC or the Shell to create a new mobile device mailbox policy.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mobile Device mailbox policy" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

## Use the EAC to create a new mobile device mailbox policy

You can create a new mobile device mailbox policy using the EAC.

> [!NOTE]
> You can only set a subset of mobile device mailbox policy settings in the EAC. To set all the mobile device mailbox policy settings, you need to use the Shell.

1. In the EAC, click **Mobile** \> **Mobile Device Mailbox Policies**, and then click **New**.

2. Use the various check boxes and drop-down lists to configure the settings for the mobile device mailbox policy.

    > [!WARNING]
    > Select <STRONG>This is the default policy</STRONG> to make the new mobile mailbox policy the default mobile mailbox policy. After you make a mobile mailbox policy the default policy, all new users will be assigned this policy automatically when they are created.

3. Click **Save**.

## Use the Shell to create a new mobile device mailbox policy

You create a new mobile device mailbox policy using the New-MobileDeviceMailboxPolicy cmdlet.

> [!WARNING]
> There are two cmdlets that can be used to create a new mobile device mailbox policy. The <STRONG>New-ActiveSyncMailboxPolicy</STRONG> cmdlet and the <STRONG>New-MobileDeviceMailboxPolicy</STRONG> cmdlets perform identical tasks. In a future version of Microsoft Exchange Server, the <STRONG>New-ActiveSyncMailboxPolicy</STRONG> cmdlet will be removed. We recommend that you update your scripts and procedures to use the <STRONG>New-MobileDeviceMailboxPolicy</STRONG> cmdlet.

1. In the Shell, run the following command.

   ```powershell
   New-MobileDeviceMailboxPolicy -Name:"Management" -AllowBluetooth:$true -AllowBrowser:$true -AllowCamera:$true -AllowPOPIMAPEmail:$false -PasswordEnabled:$true -AlphanumericPasswordRequired:$true -PasswordRecoveryEnabled:$true -MaxEmailAgeFilter:10 -AllowWiFi:$true -AllowStorageCard:$true -AllowPOPIMAPEmail:$false
   ```

## How do you know this worked?

To verify that you've successfully created a mobile device mailbox policy, use one of the following options:

1. In the EAC, click **Mobile** \> **Mobile Device mailbox policies**, and verify that your new policy is displayed in the List view.

2. In the Shell, run the following command.

   ```powershell
   Get-MobileDeviceMailboxPolicy -Identity <PolicyName>
   ```

## Edit an existing mobile device mailbox policy

If you want to edit a mobile device mailbox policy, you can use the EAC or the Shell.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mobile Device mailbox policy" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

## Use the EAC to edit a mobile device mailbox policy

You can edit a mobile device mailbox policy using the EAC.

> [!NOTE]
> You can only edit a subset of mobile device mailbox policy settings in the EAC. To edit all the mobile device mailbox policy settings, you need to use the Shell.

1. In the EAC, click **Mobile** \> **Mobile Device Mailbox Policies**.

2. Select a policy from the List view and click the **Edit** button.

3. Use the **General** and **Security** tabs to edit the mobile device mailbox policy settings.

4. Click **Save** to update the policy.

## Use the Shell to edit mobile device mailbox policy settings

You can use the Shell to edit a mobile device mailbox policy.

> [!WARNING]
> There are two cmdlets that can be used to edit a mobile device mailbox policy. The Set-ActiveSyncMailboxPolicy cmdlet and the Set-MobileDeviceMailboxPolicy cmdlets perform identical tasks. In a future version of Microsoft Exchange Server, the <STRONG>Set-ActiveSyncMailboxPolicy</STRONG> cmdlet will be removed. We recommend that you update your scripts and procedures to use the <STRONG>Set-MobileDeviceMailboxPolicy</STRONG> cmdlet.

In the Shell, run the following command.

```powershell
Set-MobileDeviceMailboxPolicy -Identity:Default -DevicePasswordEnabled:$true -AlphanumericDevicePasswordRequired:$true -PasswordRecoveryEnabled:$true -MaxEmailAgeFilter:ThreeDays -AllowWiFi:$false -AllowStorageCard:$true -AllowPOPIMAPEmail:$false -IsDefault:$true -AllowTextMessaging:$true -Confirm:$true
```

## How do you know this worked?

To verify that you've successfully edited a mobile device mailbox policy, do one of the following:

1. In the EAC, click **Mobile** \> **Mobile Device Mailbox Policy**, and then choose a specific policy. In the Details pane, you'll see a number of the policy settings listed.

2. In the Shell, run the following command.

   ```powershell
   Get-MobileDeviceMailboxPolicy -Identity <PolicyName>
   ```
