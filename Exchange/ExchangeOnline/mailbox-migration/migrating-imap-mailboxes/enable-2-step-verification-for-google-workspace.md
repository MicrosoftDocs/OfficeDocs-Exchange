---
localization_priority: Normal
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 4c70c606-5e95-4cf7-abce-cf85f1ac4179
ms.reviewer: 
description: If you want to migrate email for your Google app users to Microsoft 365 or Office 365, the users need to create an app password that you will use together with their Google Workspace password to connect to their Gmail. Before they can create an app password, you will have to allow them to turn on 2-step verification in the Google Admin console.
title: Enable 2-step verification for your Google Workspace users
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MBS150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars

---

# Enable 2-step verification for your Google Workspace users

If you want to migrate email for your Google app users to Microsoft 365 or Office 365, the users need to create an app password that you will use together with their Google Workspace password to connect to their Gmail. Before they can create an app password, you will have to allow them to turn on 2-step verification in the Google Admin console.

## Enable 2-step verification

In order for your users to create an app password, they will have to first enable 2-step verification.

 **To enable 2-step verification for your Google Workspace domain**

1. Sign in to the Google Admin console.

2. On the console choose **Security**.

    ![In the Google Admin console choose Security](../media/f0c0536d-527c-419d-b0c8-02e452fb4b4d.png)

3. On the **Security** page, choose **Basic settings**.

    ![On the Security page choose Basic settings](../media/ff1dd30f-6e45-43ca-9fd0-ed7de9e12131.png)

    And then check the check-box next to **Allow users to turn on 2-step verification**.

    ![Check Allow users to turn on 2-step verification](../media/e7870fee-90c5-47c8-8428-4130bf4c951c.png)

4. Your users can now turn on 2-step verification and create an app password as described here: [Prepare your Gmail account for connecting to Outlook and Microsoft 365 or Office 365](prepare-gmail-or-google-workspace-accounts.md).
