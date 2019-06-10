---
localization_priority: Normal
description: "Summary: Learn how to clear all data on a user's mobile phone in the Exchange admin center."
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 67ba838e-031d-4a98-b277-170683b6f520
ms.date: 7/5/2018
ms.reviewer: 
title: Perform a remote wipe on a mobile phone
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Perform a remote wipe on a mobile phone

Your users carry sensitive corporate information in their pockets every day. If one of them loses their mobile phone, your data can end up in the hands of another person. If one of your users loses their mobile phone, you can use the Exchange admin center (EAC) or the Exchange Management Shell to wipe their phone clean of all corporate and user information.

> [!NOTE]
> This topic also provides instructions for how to use Outlook on the web to perform a remote wipe on a phone. The user must be signed in to Outlook on the web to perform a remote wipe.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mobile devices" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- This procedure will clear all data on the mobile phone, including installed applications, photos, and personal information.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

- There are two types of EAS wipes:

1. Prior to EAS v16.1, remote wipe would perform a device level wipe, restoring the device to factory conditions.

2. With EAS v16.1 and later, EAS also supports account only remote wipe. When triggered, this will wipe only the EAS profile data (mailbox, contacts, and calendar). In order for this to work, the client has to support EAS v16.1 protocol. If the client doesn’t support v16.1, the wipe will fail and an error will be given.

> [!NOTE]
> If you are using Intune, you should be using Intune to trigger data removal, not Exchange. Depending on the scenario, it could be accomplished via [App Protection Policy selective wipe](https://docs.microsoft.com/intune/apps-selective-wipe), or [Device enrollment retire/wipe commands](https://docs.microsoft.com/intune/devices-wipe).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to wipe a user's phone

You can use the EAC to wipe a user's phone or cancel a remote wipe that has not yet completed.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. Select the user, and under **Mobile Devices**, choose **View details**.

3. On the **Mobile Device Details** page, select the lost mobile device, and then select **Wipe Data**.

4. Select **Save**.

## Use the Exchange Management Shell to wipe a user's phone

You can use the **Clear-MobileDevice** cmdlet in the Exchange Management Shell to wipe a user's phone.

The following command wipes the device named WM_TonySmith and sends a confirmation message to admin@contoso.com.

```
Clear-MobileDevice -Identity WM_TonySmith -NotificationEmailAddresses "admin@contoso.com"
```

## Use Outlook on the web to wipe a user's phone

Your users can use Outlook on the web to wipe their own phones.

1. In Outlook on the web, select the **Settings** icon.

2. Under **Your app settings**, select **Mail**.

3. Under **Options**, click to expand **General** if necessary, and then select **Mobile devices**.

4. Select the mobile phone.

5. Click or tap the **Wipe Device** icon.

## How do you know this worked?

There are several ways to verify that the remote wipe completed.

- Run the **Clear-MobileDevice** cmdlet with the _-NotificationEmailAddresses_ parameter configured. A message will be sent to the supplied email address when the remote wipe has completed.

- In the EAC, check the status of the mobile device. The status will change from **Wipe Pending** to **Wipe Successful**.

- In Outlook on the web, check the status of the mobile device. The status will change from **Wipe Pending** to **Wipe Successful**.
