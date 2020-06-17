---
localization_priority: Normal
description: "Summary: Learn how to clear all data on a user's mobile phone in the Exchange admin center."
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 67ba838e-031d-4a98-b277-170683b6f520
ms.reviewer: 
title: Perform a remote wipe on a mobile phone
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Perform a remote wipe on a mobile phone

Your users carry sensitive corporate information in their pockets every day. If one of them loses their mobile phone, your data can end up in the hands of another person. If one of your users loses their mobile phone, you can use the Exchange admin center (EAC) or the Exchange Management Shell to wipe their phone clean of all corporate and user information.

> [!NOTE]
> This topic also provides instructions for how to use Outlook on the web to perform a remote wipe on a phone. The user must be signed in to Outlook on the web to perform a remote wipe.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mobile devices" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!NOTE]
> Prior to EAS v16.1, remote wipe would perform a device-level wipe, restoring the device to factory conditions. With EAS v16.1 and later, EAS also supports account-only remote wipe. In order for this to work, the client must support the EAS v16.1 protocol. If the client doesn’t support v16.1, the wipe will fail and an error will be given.

> [!CAUTION]
> Exchange ActiveSync v16.1 supports two different remote wipe processes: A **Wipe Data** remote wipe and also an **Account Only Remote Wipe Device** remote wipe. There are important differences between how Outlook responds and how native mail apps on iOS and Android respond to these different wipe commands.
>
> Outlook for iOS and Outlook for Android support only the **Wipe Data** command, which wipes only data within Outlook. The Outlook app will reset and all Outlook email, calendar, contacts, and file data will be removed, but no other data is wiped from the device. The **Account Only Remote Wipe Device** command is therefore redundant and is not supported by Outlook for iOS or Android.
>
> However, if a native iOS or Android mail app is connected to Exchange and receives a **Wipe Data** command from Exchange ActiveSync, all data on the device will be wiped, including photos, personal files, and so on.
>
> If a native iOS or Android mail app is connected to Exchange and receives an **Account Only Remote Wipe Device** command from Exchange ActiveSync, only the native mail app's Exchange ActiveSync mail, calendar, and account data are wiped.
>
> These commands are designed to destroy data. Exercise caution when using them.

After the remote wipe command is requested by the administrator, the wipe happens within seconds of the Outlook app's next connection to Exchange.

Since Outlook for iOS and Android appears as a single mobile device association under a user's mobile devices in Exchange, a remote wipe command will remove data and delete sync relationships from all devices running Outlook (iPhone, iPad, Android) associated with that user.

A remote wipe action deletes the synchronization profile, so when the user adds his or her account to Outlook for iOS and Android, a new Device ID is generated and reported to Exchange on-premises.

> [!NOTE]
> If you are using Intune, you should be using Intune to trigger data removal, not Exchange. Depending on the scenario, it could be accomplished via [App Protection Policy selective wipe](https://docs.microsoft.com/intune/apps-selective-wipe), or [Device enrollment retire/wipe commands](https://docs.microsoft.com/intune/devices-wipe).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to wipe a user's phone

You can use the EAC to wipe a user's phone or cancel a remote wipe that has not yet completed.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. Select the user, and under **Mobile Devices**, choose **View details**.

3. On the **Mobile Device Details** page, select the lost mobile device, and then select **Wipe Data** (or **Account Only Remote Wipe Device** if desired).

4. Select **Save**.

## Use the Exchange Management Shell to wipe a user's phone

You can use the **Clear-MobileDevice** cmdlet in the Exchange Management Shell to wipe a user's phone.

The following command wipes the device named WM_TonySmith and sends a confirmation message to admin@contoso.com.

```PowerShell
Clear-MobileDevice -Identity WM_TonySmith -NotificationEmailAddresses "admin@contoso.com"
```

If the device connects to Exchange using a mail app other than Outlook, you can use the following command to wipe only the mail app's Exchange ActiveSync mail, calendar, and account data and leave all other data on the device intact:

```PowerShell
Clear-MobileDevice -AccountOnly -Identity WM_TonySmith -NotificationEmailAddresses "admin@contoso.com"
```

The **-AccountOnly** switch has no effect on Outlook devices because an account-only remote wipe is the only type of wipe that is supported by Outlook. See [Clear-MobileDevice](https://docs.microsoft.com/powershell/module/exchange/clear-mobiledevice) for more information.

## Use Outlook on the web to wipe a user's phone

Your users can use Outlook on the web to wipe their own phones.

1. In Outlook on the web, select the **Settings** icon.

2. Under **Your app settings**, select **Mail**.

3. Under **Options**, click to expand **General** if necessary, and then select **Mobile devices**.

4. Select the mobile phone.

5. Click or tap the **Wipe Device** icon (or the **Account Only Remote Wipe Device** icon if desired).

## Use the New Outlook on the web to wipe a user's phone

1. In Outlook on the web, select the **Settings** icon.

2. Click on **View All Outlook settings**.

3. Click **General**,  and then select **Mobile devices**.

4. Select the mobile phone.

5. Click or tap the **Wipe Device** icon (or the **Account Only Remote Wipe Device** icon if desired).

## How do you know this worked?

There are several ways to verify that the remote wipe completed.

- Run the **Clear-MobileDevice** cmdlet with the _-NotificationEmailAddresses_ parameter configured. A message will be sent to the supplied email address when the remote wipe has completed.

- In the EAC, check the status of the mobile device. The status will change from **Wipe Pending** to **Wipe Successful**.

- In Outlook on the web, check the status of the mobile device. The status will change from **Wipe Pending** to **Wipe Successful**.

> [!NOTE]
> In an Office 365-based environment, the result of a remote device wipe is not reported back to Exchange. Even when the wipe is successful, the status will display as **Pending**.
