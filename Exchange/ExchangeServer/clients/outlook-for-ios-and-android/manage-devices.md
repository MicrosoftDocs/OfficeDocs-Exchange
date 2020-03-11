---
localization_priority: Normal
description: 'Summary: This article describes how to manage mobile devices with Outlook for iOS and Android in your Exchange on-premises organization when using Basic authentication with the Exchange ActiveSync protocol.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
title: Managing devices for Outlook for iOS and Android for Exchange Server
ms.collection: exchange-server
ms.reviewer: smithre4
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Managing devices for Outlook for iOS and Android for Exchange Server

> [!IMPORTANT]
> Outlook for iOS and Android supports hybrid Modern Authentication for on-premises mailboxes which eliminates the need to leverage basic authentication. The information contained in this article only pertains to basic authentication. For more information, please see [Using hybrid Modern Authentication with Outlook for iOS and Android](https://docs.microsoft.com/Exchange/clients/outlook-for-ios-and-android/use-hybrid-modern-auth).

Microsoft recommends Exchange ActiveSync for managing the mobile devices that are used to access Exchange mailboxes in your on-premises environment. Exchange ActiveSync is a Microsoft Exchange synchronization protocol that lets mobile phones access an organization's information on a server that's running Microsoft Exchange.

This article focuses on specific Exchange ActiveSync features and scenarios for mobile devices running Outlook for iOS and Android when authenticating with Basic authentication. Complete information about the Microsoft Exchange synchronization protocol is available in [Exchange ActiveSync](../../clients/exchange-activesync/exchange-activesync.md). In addition, there is information on [the Office Blog](https://go.microsoft.com/fwlink/p/?LinkId=623922) detailing password enforcement and other benefits of using Exchange ActiveSync with devices running Outlook for iOS and Android.

## Mobile device mailbox policy

Outlook for iOS and Android supports the following mobile device mailbox policy settings in Exchange on-premises:

- Device encryption enabled

- Min password length

- Password enabled

For information on how to create or modify an existing mobile device mailbox policy, see [Mobile device mailbox policies](https://docs.microsoft.com/Exchange/clients/exchange-activesync/mobile-device-mailbox-policies).

### PIN lock and device encryption

If your organization's Exchange ActiveSync policy requires a password on mobile devices in order for users to synchronize email, Outlook will enforce this policy at the device level. This works differently between iOS devices and Android devices, based on the available controls provided by Apple and Google.

On iOS devices, Outlook checks to make sure a passcode or PIN is properly set. In the event a passcode is not set, Outlook prompts users to create a passcode in iOS settings. Until the passcode is setup, the user will be unable to access Outlook for iOS.

On Android devices, Outlook will enforce screen lock rules. In addition, Google provides controls that allow Outlook for Android to comply with Exchange policies regarding password length and complexity, and the number of allowable screen-unlock attempts before wiping the phone. Outlook for Android will also encourage storage encryption if it is not enabled, guiding users through this process with a step-by-step walkthrough.

iOS and Android devices that do not support these password security settings will not be able to connect to an Exchange mailbox.

### Device encryption

iOS devices are shipped with built-in encryption, which Outlook uses once the passcode is enabled to encrypt all the data Outlook stores locally on the iOS device. Therefore, iOS devices with a PIN are encrypted whether or not this is required by an ActiveSync policy.

Outlook for Android supports device encryption via Exchange mobile device mailbox policies. However, prior to Android 7.0, the availability and implementation of this process varies by Android OS version and device manufacturer, which allow the user to cancel out during the encryption process. With changes that Google introduced to Android 7.0, Outlook for Android is now able to enforce encryption on devices running Android 7.0 or later. Users with devices running those operating systems will not be able to cancel out of the encryption process.

Even if the Android device is unencrypted and an attacker is in possession of the device, as long as a device PIN is enabled, the Outlook database remains inaccessible. This is true even with USB debugging enabled and the Android SDK installed. If an attacker attempts to root the device to bypass the PIN to gain access to this information, the rooting process wipes all device storage and removes all Outlook data. If the device is unencrypted and rooted by the user prior to being stolen, it is possible for an attacker to gain access to the Outlook database by enabling USB debugging on the device and plugging the device into a computer with the Android SDK installed.

### Remote wipe with Exchange ActiveSync

Exchange ActiveSync enables administrators to remotely wipe devices, such as if they become compromised or lost/stolen. With Outlook for iOS and Android, a remote wipe only wipes data within the Outlook app itself and does not trigger a full device wipe.

See [Perform a remote wipe on a mobile phone](../exchange-activesync/remote-wipe.md) for more information.

## Device access policy

Outlook for iOS and Android should be enabled by default, but in some existing Exchange on-premises environments the app may be blocked for a variety of reasons. Once an organization decides to standardize how users access Exchange data and use Outlook for iOS and Android as the only email app for end users, you can configure blocks for other email apps running on users' iOS and Android devices. You have two options for instituting these blocks within Exchange on-premises: the first option blocks all devices and only allows usage of Outlook for iOS and Android; the second option allows you to block individual devices from using the native Exchange ActiveSync apps.

> [!NOTE]
> Because device IDs are not governed by any *physical device* ID, they can change without notice. When this happens, it can cause unintended consequences when device IDs are used for managing user devices, as existing 'allowed' devices may be unexpectedly blocked or quarantined by Exchange. Therefore, Microsoft recommends administrators only set mobile device access policies that allow/block devices based on device type or device model.

#### Option 1: Block all email apps except Outlook for iOS and Android

You can define a default block rule and then configure an allow rule for Outlook for iOS and Android, and for Windows devices, using the following Exchange on-premises PowerShell commands. This configuration will prevent any Exchange ActiveSync native app from connecting, and will only allow Outlook for iOS and Android.

1. Create the default block rule:

   ```powershell
   Set-ActiveSyncOrganizationSettings -DefaultAccessLevel Block
   ```

2. Create an allow rule for Outlook for iOS and Android

   ```powershell
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceModel -QueryString "Outlook for iOS and Android" -AccessLevel Allow
   ```

3. **Optional**: Create rules that allow Outlook on Windows devices for Exchange ActiveSync connectivity (WP refers to Windows Phone, WP8 refers to Windows Phone 8 and later, and WindowsMail refers to the Mail app included in Windows 10):

   ```powershell
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "WP" -AccessLevel Allow
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "WP8" -AccessLevel Allow
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "WindowsMail" -AccessLevel Allow
   ```

#### Option 2: Block native Exchange ActiveSync apps on Android and iOS devices

Alternatively, you can block native Exchange ActiveSync apps on specific Android and iOS devices or other types of devices.

1. Confirm that there are no Exchange ActiveSync device access rules in place that block Outlook for iOS and Android:

   ```powershell
   Get-ActiveSyncDeviceAccessRule | where {$_.AccessLevel -eq "Block" -and $_.QueryString -like "Outlook*"} | ft Name,AccessLevel,QueryString -auto
   ```

   If any device access rules that block Outlook for iOS and Android are found, type the following to remove them:

   ```powershell
   Get-ActiveSyncDeviceAccessRule | where {$_.AccessLevel -eq "Block" -and $_.QueryString -like "Outlook*"} | Remove-ActiveSyncDeviceAccessRule
   ```

2. You can block most Android and iOS devices with the following commands:

   ```powershell
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "Android" -AccessLevel Block
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "iPad" -AccessLevel Block
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "iPhone" -AccessLevel Block
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "iPod" -AccessLevel Block
   ```

3. Not all Android device manufacturers specify "Android" as the DeviceType. Manufacturers may specify a unique value with each release. In order to find other Android devices that are accessing your environment, execute the following command to generate a report of all devices that have an active Exchange ActiveSync partnership:

   ```powershell
   Get-MobileDevice | Select-Object DeviceOS,DeviceModel,DeviceType | Export-CSV c:\temp\easdevices.csv
   ```

4. Create additional block rules, depending on your results from Step 3. For example, if you find your environment has a high usage of HTCOne Android devices, you can create an Exchange ActiveSync device access rule that blocks that particular device, forcing the users to use Outlook for iOS and Android. In this example, you would type:

   ```powershell
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "HTCOne" -AccessLevel Block
   ```

> [!NOTE]
> The QueryString parameter does not accept wildcards or partial matches.

**Additional resources**:

- [New-ActiveSyncDeviceAccessRule](https://docs.microsoft.com/powershell/module/exchange/devices/new-activesyncdeviceaccessrule)

- [Get-MobileDevice](https://docs.microsoft.com/powershell/module/exchange/devices/get-mobiledevice)

- [Set-ActiveSyncOrganizationSettings](https://docs.microsoft.com/powershell/module/exchange/devices/set-activesyncorganizationsettings)

## Blocking Outlook for iOS and Android

Every Exchange organization has different policies regarding security and device management. If an organization decides that Outlook for iOS and Android doesn't meet their needs or is not the best solution for them, administrators have the ability to block the app. Once the app is blocked, mobile Exchange users in your organization can continue accessing their mailboxes by using the built-in mail applications on iOS and Android. 

The `New-ActiveSyncDeviceAccessRule` cmdlet has a `Characteristic` parameter, and there are three `Characteristic` options that administrators can use to block the Outlook for iOS and Android app. The options are UserAgent, DeviceModel, and DeviceType. In the two blocking options described in the following sections, you will use one or more of these characteristic values to restrict the access that Outlook for iOS and Android has to the mailboxes in your organization.

The values for each characteristic are displayed in the following table:

|**Characteristic**|**String for iOS**|**String for Android**|
|:-----|:-----|:-----|
|DeviceModel|Outlook for iOS and Android|Outlook for iOS and Android|
|DeviceType|Outlook|Outlook|
|UserAgent|Outlook-iOS-Android/1.0|Outlook-iOS-Android/1.0|

With the `New-ActiveSyncDeviceAccessRule` cmdlet, you can define a device access rule, using either the `DeviceModel` or `DeviceType` characteristic. In both cases, the access rule blocks Outlook for iOS and Android across all platforms, and will prevent any device, on both the iOS platform and Android platform, from accessing an Exchange mailbox via the app.

The following are two examples of a device access rule. The first example uses the `DeviceModel` characteristic; the second example uses the `DeviceType` characteristic.

```powershell
New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "Outlook" -AccessLevel Block
```

```powershell
New-ActiveSyncDeviceAccessRule -Characteristic DeviceModel -QueryString "Outlook for iOS and Android" -AccessLevel Block
```
