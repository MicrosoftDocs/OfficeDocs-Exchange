---
localization_priority: Normal
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: da491136-78c1-48dc-b665-29a2fbe468ac
ms.reviewer: 
manager: serdars
description: Admins can learn how to use a screen reader to configure Unified Messaging (UM) settings in the Exchange admin center (EAC) in Exchange Online.
title: Use a screen reader to configure voice mail in the Exchange admin center in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
f1.keywords:
- CSH
ms.custom: A11y_UseSR
ms.service: exchange-online
ROBOTS: NOINDEX, NOFOLLOW

---

# Use a screen reader to configure voice mail in the Exchange admin center in Exchange Online

You can use your screen reader in the Exchange admin center (EAC) to specify options and enable voicemail and other voice features for Exchange Online users. Learn more about [voicemail in Exchange Online](../voice-mail-unified-messaging/voice-mail-unified-messaging.md).

## Get started

Navigate with Internet Explorer and keyboard shortcuts, and make sure that you have the appropriate Office 365 or Microsoft 365 subscription plan and admin role to perform this task. Then, open the EAC and get started.

### Use your browser and keyboard to navigate in the EAC

Exchange Online, which includes the EAC, is a web-based application, so the keyboard shortcuts and navigation may be different from those in Exchange 2016. [Accessibility in the Exchange admin center](accessibility-in-exchange-admin-center.md).

For best results when working in the EAC in Exchange Online, use Internet Explorer as your browser. [Learn more about Internet Explorer keyboard shortcuts](https://support.microsoft.com/help/17456/).

Many tasks in the EAC require the use of pop-up windows so, in your browser, be sure to [enable pop-up windows for Office 365](https://support.microsoft.com/help/17479).

### Confirm your Office 365 or Microsoft 365 subscription plan

Exchange Online is included in several different subscription plans, but capabilities may differ by plan. If your EAC doesn't include a function described in this article, your plan might not include it.

For more information about the Exchange Online capabilities in your subscription plan, go to [What Office 365business product or license do I have?](https://support.office.com/article/f8ab5e25-bf3f-4a47-b264-174b1ee925fd) and [Exchange Online Service Description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description).

### Open the EAC, and confirm your admin role

To configure voicemail in Exchange Online, [Use a screen reader to open the Exchange admin center](use-screen-reader-to-open-exchange-admin-center.md) and check that your global administrator has assigned you to the Organization Management and UM Management admin role groups. Learn how to [Use a screen reader to identify your admin role in the Exchange admin center](use-screen-reader-to-identify-admin-role-in-exchange-admin-center.md).

### Create and configure a UM dial plan

With Unified Messaging (UM) in Office 365, administrators can manage voice messaging and email messaging from a single platform. Likewise, users can read or listen to their voicemail as well as email messages in their Inbox or use Outlook Voice Access from any telephone.

Before you configure voicemail for Exchange Online users, you or another admin must create and configure a UM dial plan; you can do both in the EAC. A UM dial plan contains configuration information related to your telephony network, and establishes a link from users' telephone extension numbers enabled for voicemail to their Outlook mailboxes. This configuration information includes extension length, dial plan protocol type, audio language, Outlook Voice Access, voicemail settings, dialing rules, and more. Learn how to [create a UM dial plan](../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md)) and [manage a UM dial plan](../voice-mail-unified-messaging/connect-voice-mail-system/manage-um-dial-plan.md).

## Enable voice features for a user

After you've configured your UM dial plan, you can enable voice features for individual user mailboxes.

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **recipients** and press Enter.

3. To move to the menu bar, press Ctrl+F6. You hear "Mailboxes, Secondary navigation link." To select the **mailboxes** link, press Enter.

4. To access the main window list view containing the list of mailboxes in your organization, press Ctrl+F6 twice.

5. To select the mailbox for which you want to enable voice features, press the Down Arrow or the Up Arrow key.

6. To access the toolbar, press Ctrl+Shift+F6. With the focus on the **Edit** button, press Enter. The **User Mailbox** dialog box opens, and the focus is on the tab names.

7. Press the Down Arrow key until the focus is on the **mailbox features** tab.

8. Tab to the **enable** link. You hear "Unified Messaging link, Enable." Press Enter. The **Enable UM Mailbox** dialog box opens.

9. The focus is on the **Browse** button for **UM mailbox policy**. Press Enter. The **Select a UM Mailbox Policy** dialog box opens, listing the UM mailbox policy names and their associated UM dial plans.

10. If there is more than one UM mailbox policy, to select the one you want to apply to the current mailbox, tab to the list of policy names and press the Down Arrow or the Up Arrow key.

11. Press Enter. The focus returns to the **Enable UM Mailbox** dialog box, which shows the selected policy in the **UM mailbox policy** text box.

   > [!TIP]
   > Depending on the UM dial plan reflected in your selected UM mailbox policy, some of the options in the following steps might be different.

12. Tab to the **Next** button and press Enter. The next page opens, in which you can specify an extension number and PIN settings for the selected user to access the mailbox in Outlook Voice Access.

13. With the focus in the **Extension number** text box, choose to leave the auto-generated extension number, or type a different one.

14. Tab to the **PIN settings**. The **Automatically generate a PIN** radio button is the default selection. To enter a different PIN, press the Down Arrow key to select the **Type a PIN** radio button. Tab to the text box and type a PIN.

15. Tab to the **Require the user to reset their PIN the first time they sign in** check box. This check box is the default selection. To clear the check box, press the Spacebar.

16. Tab to the **Finish** button and press Enter. An email message containing the PIN and the access number for Outlook Voice Access is sent to the mailbox owner.

17. The focus returns to the **User Mailbox** dialog box, where the **Phone and Voice Features** section now shows that Unified Messaging is enabled.

18. Tab to the **Save** button and press Enter. The **mailboxes** list view has the focus.
