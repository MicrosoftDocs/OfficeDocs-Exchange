---
localization_priority: Normal
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 775bb2a2-5ada-489b-944f-422d56d94fbf
ms.reviewer: 
manager: serdars
description: Admins can learn how to use a screen reader to enable or disable mailbox archiving in the Exchange admin center (EAC) in Exchange Online.
title: Use a screen reader to archive mailbox items in the Exchange admin center in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
f1.keywords:
- CSH
ms.custom: A11y_UseSR
ms.service: exchange-online

---

# Use a screen reader to archive mailbox items in the Exchange admin center in Exchange Online

You can use your screen reader in the Exchange admin center (EAC) to enable or disable archiving of items in an Exchange Online mailbox. You can also use your screen reader in the EAC to apply retention policies to mailboxes. Learn more about the [archive mailboxes in Exchange Online](https://docs.microsoft.com/microsoft-365/compliance/enable-archive-mailboxes).

## Get started

Navigate with Internet Explorer and keyboard shortcuts, and make sure that you have the appropriate Office 365 or Microsoft 365 subscription plan and admin role to work in the EAC. Then, open the EAC and get started.

For more information about creating distribution groups, refer to Use a screen reader to create a new distribution group in the Exchange admin center.

### Use your browser and keyboard to navigate in the EAC

Exchange Online, which includes the EAC, is a web-based application, so the keyboard shortcuts and navigation may be different from those in Exchange 2016. [Accessibility in the Exchange admin center](accessibility-in-exchange-admin-center.md).

For best results when working in the EAC in Exchange Online, use Internet Explorer as your browser. [Learn more about Internet Explorer keyboard shortcuts](https://support.microsoft.com/help/17456/).

Many tasks in the EAC require the use of pop-up windows so, in your browser, be sure to [enable pop-up windows for Office 365](https://support.microsoft.com/help/17479).

### Confirm your Office 365 or Microsoft 365 subscription plan

Exchange Online is included in several different subscription plans. But capabilities may differ by plan. If your EAC doesn't include a function described in this article, your plan might not include it.

For more information about the Exchange Online capabilities in your subscription plan, go to [What Office 365 business product or license do I have?](https://support.office.com/article/f8ab5e25-bf3f-4a47-b264-174b1ee925fd) and [Exchange Online Service Description.](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description).

### Open the EAC, and confirm your admin role

To complete the tasks covered in this topic, [Use a screen reader to open the Exchange admin center](use-screen-reader-to-open-exchange-admin-center.md) and check that your global administrator has assigned you to the Organization Management and Records Management admin role groups. [Use a screen reader to identify your admin role in the Exchange admin center](use-screen-reader-to-identify-admin-role-in-exchange-admin-center.md).

## Enable mailbox archiving for a user

With mailbox archiving in Exchange Online, also called "in-place archiving," users get additional mailbox storage space. When enabled, archive mailboxes are accessible through Outlook and Outlook on the web, and offer a convenient alternate repository for old email messages.

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **recipients** and press Enter.

3. To move to the menu bar, press Ctrl+F6. You hear "Mailboxes, Secondary navigation link." To select the **mailboxes** link, press Enter.

4. To search for the user for whom you want to enable archiving, press Ctrl+F6 and then press the Tab key until you hear "Search button." Press Enter.

5. Type all or part of the user's name and press Enter.

6. Press Ctrl+F6 until you hear the name of the user in the search results list. If the search results list includes multiple names, press the Down Arrow key or the Up Arrow key until you hear the name you want.

7. To move to the details pane, press Ctrl+F6. You hear "Unified Messaging link."

8. Press the Tab key about six times until you hear "Archiving link, Enable..

**Tip**: If the user is already enabled for archiving, you hear "Archiving link, Disable..

9. Press Enter. You hear "Are you sure you want to enable the archive?" With the focus on the **Yes** button, press Enter.

   > **Tip**: If you want to enable archiving for additional users, move the focus back to the list of mailboxes by pressing Ctrl+Shift+F6. Select the name you want by pressing the Down Arrow key or the Up Arrow key, and repeat steps 7 through 9.

    **Note**: For more information, go to [Enable archive mailboxes in the Security & Compliance Center](https://docs.microsoft.com/microsoft-365/compliance/enable-archive-mailboxes).

## Disable mailbox archiving for a user

If you disable a user's archive, the existing content is retained for 30 days. This means if you re-enable the archive within that 30 days, all existing content will still be intact. After 30 days, however, all information is permanently deleted, and if you enable the archive after this time, a new archive mailbox is created.

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **recipients** and press Enter.

3. To move to the menu bar, press Ctrl+F6. You hear "Mailboxes, Secondary navigation link." To select the **mailboxes** link, press Enter.

4. To search for the user for whom you want to enable archiving, press Ctrl+F6 and then press the Tab key until you hear "Search button." Press Enter.

5. Type all or part of the user's name and press Enter.

6. Press Ctrl+F6 until you hear the name of the user whose mailbox archiving you want to disable in the search results list. If the search results list includes multiple names, press the Down Arrow key or the Up Arrow key until you hear the name you want.

7. To move to the details pane, press Ctrl+F6. You hear "Unified Messaging link."

8. Press the Tab key about six times until you hear "Archiving link, Disable..

9. Press Enter. You hear "Are you sure you want to disable this archive?" With the focus on the **Yes** button, press Enter.

## Apply a retention policy to a user

The messaging records management (MRM) feature in Exchange Online helps you manage the life cycle of your organization's email; it allows you to set retention policies. Retention policies specify when certain types of mailbox items (including regular email messages, deleted items, and junk mail) should be moved, archived, or deleted. Exchange Online automatically applies the Default MRM Policy when you create a new mailbox with an archive or when you enable an archive for an existing mailbox user.

 **Note**: You can customize the Default MRM Policy by adding or removing retention tags or by modifying tag settings. You can also replace the default policy with any retention policies you create. To view, edit, or create a retention policy, on the EAC primary navigation pane, select the compliance management link and then, on the menu bar, select the retention policies link. [Learn more about retention policies](https://go.microsoft.com/fwlink/p/?LinkId=799151).

You can apply the same retention policy to all users, or you can apply different policies to certain users.

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **recipients** and press Enter.

3. To move to the menu bar, press Ctrl+F6. You hear "Mailboxes, Secondary navigation link." To select the **mailboxes** link, press Enter.

4. To search for the user for whom you want to enable archiving, press Ctrl+F6 and then press the Tab key until you hear "Search button." Press Enter.

5. Type all or part of the user's name and press Enter.

6. Press Ctrl+F6 until you hear the name of the user in the search results list. If the search results list includes multiple names, press the Down Arrow key or the Up Arrow key until you hear the name you want. Press Enter.

7. In the **Edit User Mailbox** dialog box which opens, with the focus on the tab names, press the Down Arrow key until the focus is on the **mailbox features** tab.

8. Tab to the **Retention policy** combo box. **Default MRM Policy** is the default entry. Press the Down Arrow key or the Up Arrow key to move through the available policies. Select the policy you want for this user.

9. Tab to the **Save** button and press Enter. The **mailboxes** list view has the focus again.

## Accessibility information

The [Microsoft Accessibility](https://go.microsoft.com/fwlink/p/?LinkID=21487) website provides more information about assistive technology. A free monthly electronic newsletter is available to help you stay current with accessibility topics about Microsoft products. To subscribe, visit the [Microsoft Accessibility Update Newsletter Subscription](https://go.microsoft.com/fwlink/p/?LinkID=14920) page.

### Technical support for customers with disabilities

Microsoft wants to provide the best possible experience for all our customers. If you have a disability or have questions related to accessibility, please contact the [Microsoft Disability Answer Desk](https://go.microsoft.com/fwlink/p/?LinkID=518252) for technical assistance.

The Disability Answer Desk support team is trained in using many popular assistive technologies and can offer assistance in English, Spanish, French, and American Sign Language. Please visit the [Microsoft Disability Answer Desk](https://go.microsoft.com/fwlink/p/?LinkID=518252) site to find the contact details for your region.
