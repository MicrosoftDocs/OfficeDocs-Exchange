---
localization_priority: Normal
description: The Clutter feature uses Inbox notifications to invite users and to send status messages. The default branding used for these notifications is Outlook, but you can modify the branding for your organization.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 34bb5558-be1b-4ed2-a6c1-cb5031a33317
ms.reviewer: 
f1.keywords:
- NOCSH
title: Change the branding of Clutter notifications
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Change the branding of Clutter notifications

> [!TIP]
> [Focused Inbox](https://github.com/MicrosoftDocs/microsoft-365-docs/blob/public/microsoft-365/admin/setup/configure-focused-inbox.md) is going to replace Clutter. Learn more: [Update on Focused Inbox and our plans for Clutter](https://techcommunity.microsoft.com/t5/Outlook-Blog/Update-on-Focused-Inbox-and-our-plans-for-Clutter/ba-p/136448).

The Clutter feature uses Inbox notifications to invite users and to send status messages. The default branding used for these notifications is Outlook, but you can modify the branding for your organization.

## Change the branding of Clutter notifications (new EAC)

This article describes how to change the branding of Clutter notifications to match that of your school, business, or organization.

> [!NOTE]
> For more information about the types of Clutter notifications that end users in your organization receive, see [Clutter notifications in Outlook](clutter-notifications-in-outlook.md).

To begin, you will need to sign in to Microsoft 365 or Office 365 with your work or school account.

1. Once signed in to Microsoft 365 or Office 365, go to the Microsoft 365 admin center.

2. Click to expand **Users**, then select **Active Users**.

3. Select **Add** ![Add](../../media/ITPro_EAC_AddIcon.png) to add a user. The **Create a new user account** dialog will open.

4. In the **Create a new user account** dialog, enter a **Display name** and a **username**. The display name will appear in the Sender field for all Clutter notifications sent to your users. A new temporary password is generated for the new user account. Click **Create** to create the account.

5. Go to the new Exchange admin center (EAC).

6. Click **Recipients**, and then click **Mailboxes**.

7. Select the user you just created. A details pane will be shown.

8. Under **Mailbox** settings \> **Email addresses**, click **Manage email address types**. 

9. In the **Manage email address types** display pane, click ![Add](../../media/ITPro_EAC_AddIcon.png) **Add email address type**  to add an email address to the new user account.

10. In the **new email address** dialog, select SMTP as the email address type, and then, in the **Email address:\*** box, type the following: **7a694ec2-b7c9-41eb-b562-08fd2b277ae0@[your default domain]**, where [your default domain] is the domain that your organization uses. For most organizations, this would be **[your domain name].onmicrosoft.com**. When finished, click **OK**.

11. Back in the **Manage email address types** dialog, click **Save** to associate the new email address with the user account. All Clutter notifications sent to end users in your organization will now originate from this account.


## Change the branding of Clutter notifications (Classic EAC)

This article describes how to change the branding of Clutter notifications to match that of your school, business, or organization.

> [!NOTE]
> For more information about the types of Clutter notifications that end users in your organization receive, see [Clutter notifications in Outlook](clutter-notifications-in-outlook.md).

To begin, you will need to sign in to Microsoft 365 or Office 365 with your work or school account.

1. Once signed in to Microsoft 365 or Office 365, go to the Microsoft 365 admin center.

2. Click to expand **Users**, then select **Active Users**.

3. Select **Add** ![Add](../../media/ITPro_EAC_AddIcon.png) to add a user. The **Create a new user account** dialog will open.

4. In the **Create a new user account** dialog, enter a **Display name** and a **username**. The display name will appear in the Sender field for all Clutter notifications sent to your users. A new temporary password is generated for the new user account. Click **Create** to create the account.

5. Go the Exchange admin center.

6. Click **recipients**, and then click **mailboxes**.

7. Select the user you just created, and then click the pencil icon to edit the account, as shown in the following example.

   ![Picture of the Exchange admin center when creating your branding mailbox for Clutter.](../../media/98be1aee-ae96-4406-bf47-91336c62b5c7.png)

8. In the user account dialog, click **Email address**, and then click **Add** ![Add](../../media/ITPro_EAC_AddIcon.png) to add an email address to the new user account.

   ![Picture of the user dialog box, which is used to add a new email address to the user account.](../../media/1bfb758a-c1a5-4314-aa0f-f34655bb501f.png)

9. In the **new email address** dialog, select SMTP as the email address type, and then, in the **Email address** box, type the following: **7a694ec2-b7c9-41eb-b562-08fd2b277ae0@[your default domain]**, where [your default domain] is the domain that your organization uses. For most organizations, this would be **[your domain name].onmicrosoft.com**.

   When finished, click **OK**.

   ![Picture of the new email address dialog, with the email address you need to enter to rebrand Clutter notifications.](../../media/28371e1f-964a-4ed9-8e75-4145c58adb2f.png)

10. Back in the user account dialog, click **save** to associate the new email address with the user account. All Clutter notifications sent to end users in your organization will now originate from this account.

## Change the branding of Clutter notifications using PowerShell

You can also create a new shared mailbox as the branding mailbox using PowerShell. Follow these steps.

1. [Connect to Exchange PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

2. Type the following commands:

   ```PowerShell
   New-Mailbox -Shared -Name branding@contoso.com -DisplayName "Branding Clutter Mailbox" -Alias branding
   Set-Mailbox "IT Admin" -EmailAddresses SMTP: branding@contoso
   ```
