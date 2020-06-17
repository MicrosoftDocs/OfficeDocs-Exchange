---
title: 'Computer must be restarted before Setup can continue'
TOCTitle: The computer needs to be restarted before Setup can continue
ms:assetid: d5c73280-4e54-473a-b328-9673af11e2c0
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.rebootpending(v=EXCHG.150)
ms:contentKeyID: 46629129
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# The computer needs to be restarted before Setup can continue

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup can't continue because it detected that the local computer needs to be restarted to complete the installation of other programs or Windows updates.

## Why is this happening?

When programs and Windows updates are installed, they make changes to files that are stored on your computer. Sometimes, during installation, a program or Windows update needs to make a change to a file that's in use. When this happens, you need to restart the computer before any other programs, such as Exchange 2013, can be installed. Exchange Setup requires that all other installations have finished so it can verify that all of the prerequisites it needs to work properly have been installed.

Sometimes, the installation of a previous program or Windows update might not complete successfully. When this happens, it can leave behind changes that make Windows and other programs think a restart is needed. Unfortunately, because the failed installation never cleans up these changes, the installation of Windows updates and other programs can be blocked. You'll continue to see this error each time you run Exchange Setup if this happens.

## How do I fix it?

In most cases, you just need to restart the computer to get past this error. There are times, however, when you might get this error again even though you've already restarted the computer. This can happen when the installation of a program or Windows update needs to make additional changes and those changes also require that the computer be restarted. If you see this error after you restart your computer, try restarting it again.

If you've restarted the computer more than two or three times, and you're still seeing this error, try and reinstall any programs or Windows updates you've installed recently. This might allow a failed installation to complete successfully.

If you *still* receive this error after multiple restarts and reinstalling recent programs or Windows updates, we recommend that you contact Microsoft Customer Service and Support. They'll help you find the reason why Windows and other programs think your computer needs to be restarted. To contact Microsoft support, go to [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and select **Servers** \> **Exchange Server**.

> [!WARNING]
> Even though it can be tempting to do so, we strongly recommend that you don't attempt to work around this issue by manually deleting or changing keys or values in the Windows Registry. While doing so might fix this issue now, it might cause issues later on. This is especially important if the failed installation was a Windows update.
