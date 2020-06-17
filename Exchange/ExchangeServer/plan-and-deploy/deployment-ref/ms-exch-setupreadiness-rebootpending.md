---
localization_priority: Normal
description: Exchange Server 2016 or Exchange 2019 Setup can't continue because the local computer needs to be restarted to complete the installation of other programs or Windows updates.
ms.topic: reference
author: mattpennathe3rd
ms.custom:
- ms.exch.setupreadiness.RebootPending
ms.author: v-mapenn
ms.assetid: d5c73280-4e54-473a-b328-9673af11e2c0
ms.reviewer: 
title: The computer needs to be restarted before Setup can continue [RebootPending]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: Developer
ms.prod: exchange-server-it-pro
manager: serdars

---

# The computer needs to be restarted before Setup can continue [RebootPending]

Exchange Setup can't continue because it detected a pending reboot to complete the installation of other programs or Windows updates.

## Why is this happening?

When programs and Windows updates are installed, they make changes to files that are stored on your computer. Some programs or updates need to modify or replace files that are currently in use. When this happens, you need to restart the computer before other programs can be installed.

If the installation of a previous program or Windows update didn't complete successfully, Windows and other programs might think a restart is required. You'll continue to see this error each time you run Exchange Setup if this happens (the failed installation can't fix the condition that indicates a restart is required).

## How do I fix it?

Typically, you only need to restart the server to get past this error, but you might get this error again after a restart (for example, additional program or Windows updates also require a restart). Try restarting the server again.

If you see this error after you've restarted the server more than two or three times, try reinstalling any programs or Windows updates that you've installed recently. This might allow a failed installation to complete successfully.

If you *still* receive this error after multiple restarts and reinstalling recent programs or Windows updates, we recommend that you contact Microsoft Customer Service and Support. They'll help you find the reason why Windows and other programs think your computer needs to be restarted. To contact Microsoft support, go to [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and select **Servers** \> **Exchange Server**.

> [!CAUTION]
> Although it's tempting, we strongly recommend that you don't attempt to work around this issue by manually deleting or changing registry keys or values. Although you might fix this issue now, manually modifying the registry might cause issues later on. This is especially important if the failed installation was a Windows update.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
