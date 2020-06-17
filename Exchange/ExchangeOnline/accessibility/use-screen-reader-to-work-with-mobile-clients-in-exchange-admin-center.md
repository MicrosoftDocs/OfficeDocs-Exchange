---
localization_priority: Normal
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 194c5bf8-845c-4804-82dd-e950ee74dfcf
ms.reviewer: 
manager: serdars
description: Admins can learn how to use a screen reader to manage mobile devices in the Exchange admin center (EAC) in Exchange Online.
title: Use a screen reader to work with mobile clients in the Exchange admin center in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
f1.keywords:
- CSH
ms.custom: A11y_UseSR
ms.service: exchange-online

---

# Use a screen reader to work with mobile clients in the Exchange admin center in Exchange Online

You can use your screen reader in the Exchange admin center (EAC) to enable the use of mobile devices for users of Exchange Online, who can then access information in their Office 365 mailboxes through mobile phones and tablets. [Learn more about clients and mobile in Exchange Online](../clients-and-mobile-in-exchange-online/clients-and-mobile-in-exchange-online.md).

## Get started

Navigate with Internet Explorer and keyboard shortcuts, and make sure that you have the appropriate Office 365 or Microsoft 365 subscription plan and admin role to work in the EAC. Then, open the EAC and get started.

### Use your browser and keyboard to navigate in the EAC

Exchange Online, which includes the EAC, is a web-based application, so the keyboard shortcuts and navigation may be different from those in Exchange 2016. [Accessibility in the Exchange admin center](accessibility-in-exchange-admin-center.md).

For best results when working in the EAC in Exchange Online, use Internet Explorer as your browser. [Learn more about Internet Explorer keyboard shortcuts](https://support.microsoft.com/help/17456/).

Many tasks in the EAC require the use of pop-up windows so, in your browser, be sure to [enable pop-up windows for Office 365](https://support.microsoft.com/help/17479).

### Confirm your Office 365 or Microsoft 365 subscription plan

Exchange Online is included in several different subscription plans. But capabilities may differ by plan. If your EAC doesn't include a function described in this article, your plan might not include it.

For more information about the Exchange Online capabilities in your subscription plan, go to [What Microsoft 365 Apps for business product or license do I have?](https://support.office.com/article/f8ab5e25-bf3f-4a47-b264-174b1ee925fd
) and [Exchange Online Service Description.](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description
).

### Open the EAC, and confirm your admin role

To complete the tasks covered in this topic, [Use a screen reader to open the Exchange admin center](use-screen-reader-to-open-exchange-admin-center.md) and check that your global administrator has assigned you to the Organization Management and Records Management admin role groups. [Use a screen reader to identify your admin role in the Exchange admin center](use-screen-reader-to-identify-admin-role-in-exchange-admin-center.md).

## Configure mobile device mailbox policies and access

You can use the EAC to create mobile device mailbox policies that apply a common set of rules or security settings to a collection of users. If you don't create your own mobile device mailbox policy, the default policy is applied, which includes the following settings:

- Allow mobile devices that don't fully support policies to synchronize.

- Outlook Web App (OWA) for Devices supports all password policies and won't block any devices.

- A password is optional.

- Device encryption is not required.

To view, edit, or create a mobile device mailbox policy, on the EAC primary navigation pane, select the **mobile** link and then, on the menu bar, select the **mobile device mailbox policies** link. Learn more about the options you can set for [mobile device mailbox policies](../clients-and-mobile-in-exchange-online/clients-and-mobile-in-exchange-online.md).

You can also specify Exchange ActiveSync access settings, maintain a list of quarantined mobile devices, and set up device access rules. To do this, on the EAC primary navigation pane, select the **mobile** link and then, on the menu bar, select the **mobile device access** link.

## Enable Exchange ActiveSync and Outlook on the web for users

Exchange ActiveSync is an Exchange synchronization protocol which allows mobile phones to access your organization's Exchange server. With Exchange ActiveSync, recipients can use their mobile devices to access their email, calendar, contacts, and tasks. They can also continue to access this information while working offline. Learn more about [Exchange ActiveSync](../clients-and-mobile-in-exchange-online/exchange-activesync/exchange-activesync.md).

With Outlook on the web (formerly known as Outlook Web App), users can access their Exchange mailbox from almost any web browser, including from a browser on their mobile devices. [Learn more about Outlook on the web](../clients-and-mobile-in-exchange-online/outlook-on-the-web/outlook-on-the-web.md).

### Enable Exchange ActiveSync and Outlook on the web for an individual user

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **recipients** and press Enter.

3. To move to the menu bar, press Ctrl+F6. You hear "Mailboxes, Secondary navigation link." To select the **mailboxes** link, press Enter.

4. To search for the user for whom you want to enable Exchange ActiveSync, press Ctrl+F6 and then press the Tab key until you hear "Search button." Press Enter.

5. Type all or part of the user's name and press Enter.

6. Press Ctrl+F6 until you hear the name of the user in the search results list. If the search results list includes multiple names, press the Down Arrow key or the Up Arrow key until you hear the name you want.

7. To move to the details pane, press Ctrl+F6. You hear "Unified Messaging link, Enable."

8. Press the Tab key. You hear "Mobile devices link, Enable Exchange ActiveSync..

   > [!TIP]
   > If the user is already enabled for Exchange ActiveSync, you hear "Disable Exchange ActiveSync..

9. Press Enter. You hear "Are you sure you want to enable Exchange ActiveSync?" With the focus on the **Yes** button, press Enter.

10. Press the Tab key. You hear "Mobile devices link, Enable OWA for Devices."

    > [!TIP]
    > If the user is already enabled for OWA for Devices, you hear "Disable OWA for Devices..

11. Press Enter. You hear "Are you sure you want to enable OWA for Devices?" With the focus on the **Yes** button, press Enter.

    > [!TIP]
    > If you want to enable Exchange ActiveSync and Outlook on the web for additional users, press Ctrl+Shift+F6 to move the focus back to the list of users. Press the Down Arrow key or the Up Arrow key until you hear the name you want, and repeat steps 7 through 11.

### Enable Exchange ActiveSync and Outlook on the web for multiple users at once

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **recipients** and press Enter.

3. To move to the menu bar, press Ctrl+F6. You hear "Mailboxes, Secondary navigation link." To select the **mailboxes** link, press Enter.

4. Press Ctrl+F6 twice to move to the list of users. Press the Down Arrow key or the Up Arrow key to move to the first adjacent user. Hold down the Shift key and press the Down Arrow key or the Up Arrow key to select more adjacent users.

   > [!TIP]
   > To select all users, press Ctrl+A.

5. Repeatedly press the Tab key until the **Bulk Edit** details pane has the focus and you hear "Bulk Edit..

6. Press the Tab key until you hear "Enable link." Press Enter.

7. An alert asks "Are you sure you want to enable Outlook on the web for all the selected recipients?" With the focus on the **OK** button, press Enter.

8. Press the Tab key about 10 times until you hear "Show link." Press the Tab key once more. You hear "Enable link." Press Enter.

9. An alert asks "Are you sure you want to enable Exchange ActiveSync for all the selected recipients?" With the focus on the **OK** button, press Enter.
